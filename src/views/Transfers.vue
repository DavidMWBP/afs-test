<template>
  <div class="transfers">
    <h1 class="title is-1">Transfers</h1>
    <label
      >Search
      <input v-model="searchTerms" />
    </label>
      <button class="edit-btn" @click="updateTransfers">
        Update transfers
      </button>
    <div class="transfersList">
      <transfer-row
        :key="transfer.transactionIdentifier"
        v-for="transfer in searchedTransfers"
        :transfer="transfer"
      />
    </div>
  </div>
</template>

<script lang="ts">
import {
  Component,
  Vue,
} from 'vue-property-decorator';

import transfers from '@/assets/data';
import TransferRow from '@/components/transferRow.vue';
import { Transaction } from '@/types/types';

@Component({
  name: "Transfers",
  components: { TransferRow },
})
export default class Transfers extends Vue {
  searchTerms = "";
  transfers = transfers;
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  get searchedTransfers() {
    if (this.searchTerms) {
      // custom search, should be improved upon
      const searchArray: Transaction[] = [];
      this.transfers.forEach((transfer: Transaction) => {
        if (
          transfer.type.toLowerCase().includes(this.searchTerms.toLowerCase()) ||
          transfer.recordDate?.toLowerCase().includes(this.searchTerms.toLowerCase())
        ) {
          searchArray.push(transfer);
        }
      });
      return searchArray;
    }
    return this.transfers;
  }

  updateTransfers(): void {
    /**
     * The reason this method wasn't working is because Vue cannot detect property additions and deletions on plain JavaScript objects.
     * By using Vue.set, we are notifying Vue to add the property forgottenProperty to the transfer object and at the same time triggering a reactivity update on the component, which in turn causes the component to re-render with the updated transfer objects.
     */
    this.transfers.forEach((transfer, index) => {
      Vue.set(this.transfers, index, {
        ...transfer,
        forgottenProperty: `Important data: ${(Math.random() * 100000000).toString().slice(1, 8)}`,
      });
    });

    this.transfers[0] = {
      splitFactor: null,
      exDate: null,
      amount: 10000,
      companyId: "568fa387-43d1-499a-bba2-25089f5a881a",
      notes: null,
      pricePerShare: null,
      recordDate: "2021-07-01",
      securityClassId: "ab983cfe-a932-4e25-98ea-f5928a839fe1",
      securityClass: { name: "Common" },
      state: "OLD",
      toSecurityHolderId: "dd971e7f-386b-45dd-93e1-666fbeed0a55",
      toSecurityHolder: {
        fullName: "Jeff Dunlap",
        type: "PERSON",
      },
      transactionIdentifier: "41095fdb-6b52-4257-aef8-dc523d782e53",
      positionWithinDay: 3,
      type: "ISSUE_STOCK",
    };
  }
}
</script>

<style scoped lang="scss">
  .edit-btn {
    margin: 2rem;
  }

  .transfersList {
    display: flex;
    background-color: #535a74;
    padding: 1rem;
    flex-direction: column;
    align-items: center;
  }
</style>
