<template>
  <div>
    <div class="px-4 pb-3 border-bottom d-flex">
      <UiButton
        :class="!addressIsValid && 'border-red'"
        class="width-full"
        style="max-width: 430px;"
      >
        <input
          v-model.trim="form.address"
          @input="setAddress"
          type="text"
          class="input width-full"
          placeholder="0x123..."
        />
      </UiButton>
      <div class="flex-auto text-right">
        <UiButton
          :key="i"
          v-for="(c, i) in contracts"
          @click="setContract(i)"
          :class="form.contract === i && 'button--active'"
          class="mr-2 mb-2"
        >
          {{ startCase(c.contractName) }}
        </UiButton>
      </div>
    </div>
    <div v-if="contract">
      <div v-if="!form.command">
        <a
          :key="i"
          v-for="(c, i) in commands"
          @click="setCommand(c.name)"
          :class="form.command === c.name && 'button--active'"
          class="px-4 py-3 border-bottom d-block"
        >
          {{ startCase(c.name) }}
        </a>
      </div>
    </div>
    <div v-if="command" class="d-flex height-full">
      <form @submit.prevent="handleSubmit" class="col-6 p-4">
        <h3 v-text="startCase(command.name)" class="mb-3" />
        <div style="max-width: 500px;">
          <Command :command="command" v-model="form.params" />
          <UiButton
            :loading="loading"
            :disabled="!form.address || !addressIsValid"
            type="submit"
            class="width-full button--submit mt-2"
          >
            Submit
          </UiButton>
        </div>
      </form>
      <div class="col-6 p-4 bg-gray">
        <div class="mb-3">
          <h3 class="mb-2" v-text="'Command'" />
          <div v-text="command.name" />
        </div>
        <h3 class="mb-2" v-text="'Params'" />
        <code><pre v-text="JSON.stringify(form.params, null, 2)"/></code>
      </div>
    </div>
  </div>
</template>

<script>
import contracts from '@/sign/abi';
import { Contract } from '@ethersproject/contracts';
import { Web3Provider } from '@ethersproject/providers';
import { getAddress, isAddress } from '@ethersproject/address';
import startCase from 'lodash/startCase';

const setup = {
  CRPFactory: {
    addresses: [
      {
        address: '0x17e8705E85aE8E3df7C5E4d3EEd94000FB30C483',
        name: 'Balancer smart pool factory'
      }
    ]
  },
  IERC20: {
    addresses: [
      {
        address: '0x1528F3FCc26d13F7079325Fb78D9442607781c8C',
        name: 'DAI'
      },
      {
        address: '0xd0A1E359811322d97991E03f863a0C30C2cF029C',
        name: 'WETH'
      }
    ]
  }
};

export default {
  data() {
    return {
      loading: false,
      form: {
        address: this.$route.query.address,
        contract: this.$route.query.contract,
        command: this.$route.query.command,
        params: []
      },
      contracts,
      setup
    };
  },
  computed: {
    addressIsValid() {
      return !this.form.address || isAddress(this.form.address);
    },
    contract() {
      return this.contracts[this.form.contract];
    },
    commands() {
      if (!this.contract) return [];
      return this.contract.abi.filter(
        command =>
          command.type === 'function' &&
          !['view', 'pure'].includes(command.stateMutability)
      );
    },
    command() {
      return this.commands.filter(
        command => command.name === this.form.command
      )[0];
    }
  },
  methods: {
    startCase,
    setContract(key) {
      this.form.command = undefined;
      this.form.params = [];
      this.form.address = this.form.address ? this.form.address : undefined;
      if (key === this.form.contract) return;
      this.form.contract = key;
      this.$router.push({
        path: this.$router.currentRoute.path,
        query: {
          address: this.form.address,
          contract: key,
          command: this.form.command
        }
      });
    },
    setAddress() {
      this.$router.push({
        path: this.$router.currentRoute.path,
        query: {
          address: this.form.address,
          contract: this.form.contract,
          command: this.form.command
        }
      });
    },
    setCommand(key) {
      if (key === this.form.command) return;
      this.form.command = key;
      this.$router.push({
        path: this.$router.currentRoute.path,
        query: {
          address: this.form.address,
          contract: this.form.contract,
          command: key
        }
      });
    },
    async handleSubmit() {
      this.loading = true;
      try {
        const web3 = new Web3Provider(this.$auth.provider);
        const signer = web3.getSigner();
        const contract = new Contract(
          getAddress(this.form.address),
          this.contract.abi,
          web3
        );
        const contractWithSigner = contract.connect(signer);
        const overrides = {};
        const gasLimitNumber = await contractWithSigner.estimateGas[
          this.command.name
        ](...this.form.params, overrides);
        const gasLimit = gasLimitNumber.toNumber();
        overrides.gasLimit = Math.floor(gasLimit * 1.1);
        const tx = await contractWithSigner[this.command.name](
          ...this.form.params,
          overrides
        );
        await tx.wait();
        console.log('Success', tx);
      } catch (e) {
        console.log('Error', e);
      }
      this.loading = false;
    }
  }
};
</script>