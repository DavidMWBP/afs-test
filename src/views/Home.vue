<template>
  <div class="home">
    <h1>This is a table with some important data</h1>
    <b-table :data="tableData" :columns="columns"></b-table>
    <button type="button" @click="showModal">Add security class</button>

    <Modal
      v-show="isModalVisible"
      @close="closeModal"
      @add-security-class="addSecurityClassItem"
    />
  </div>
</template>

<script lang="ts">
import {
  Component,
  Vue,
} from 'vue-property-decorator';

import { TableData } from '@/types/types';

import Modal from '../components/modal.vue';

@Component({
  components: {
    Modal,
  },
})
export default class Home extends Vue {
  tableData: TableData[] = [];
  columns = [
    {
      label: "Security class",
      field: "name",
    },
    {
      label: "Authorized amount",
      field: "authorizedAmount",
    },
    {
      label: "Issued amount",
      field: "issuedAmount",
    },
    {
      label: "Authorized Capital",
      field: "authorizedCapital",
    },
    {
      label: "Issued capital",
      field: "issuedCapital",
    },
  ];
  loading = false;
  isModalVisible = false;

  // mounted works fine if your ide complains about it
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  async mounted() {
    try {
      const data = await this.getData()
      const totals = this.calculateTotals(data);

      this.loading = true;

      const mappedData = data.map((dataItem: TableData) => {
        return {
          ...dataItem,
          randomNumber: Math.random(),
        };
      });

      this.tableData = [...mappedData, totals];
      this.loading = false;

    } catch (error) {
      console.log(error, "This is not good");
    }
  }
  
  showModal(): void {
    this.isModalVisible = true;
  }

  closeModal(): void {
    this.isModalVisible = false;
  }

  addSecurityClassItem(newSecurityClassItem: TableData): void {
    this.tableData.pop() // Removes total row
    this.tableData.push(newSecurityClassItem);
    this.tableData.push(this.calculateTotals(this.tableData));
    this.closeModal();
}

  async getData(): Promise<TableData[]> {
    return [
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series A",
        nominalValue: 5,
        authorizedAmount: 1500,
        issuedAmount: 500,
        authorizedCapital: 7550,
        issuedCapital: 2500,
      },
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series B",
        nominalValue: 10,
        authorizedAmount: 15000,
        issuedAmount: 5000,
        authorizedCapital: 150000,
        issuedCapital: 50000,
      },
      {
        id: "fd78c11b-e3d2-455a-99b0-49907a75c463",
        name: "Series C",
        nominalValue: 1,
        authorizedAmount: 96876,
        issuedAmount: 61760,
        authorizedCapital: 96876,
        issuedCapital: 61760,
      },
      {
        id: "d8654cb0-8986-4fbc-b969-025e514cb934",
        name: "Series D",
        nominalValue: 1,
        authorizedAmount: 10110,
        issuedAmount: 1100,
        authorizedCapital: 10110,
        issuedCapital: 1100,
      },
    ];
  }

  calculateTotals(data: TableData[]): TableData {
    const totals: TableData = {
      id: "totals",
      name: "Total",
      nominalValue: 0,
      authorizedAmount: data.reduce((sum, item) => sum + Number(item.authorizedAmount), 0),
      issuedAmount: data.reduce((sum, item) => sum + Number(item.issuedAmount), 0),
      authorizedCapital: data.reduce((sum, item) => sum + Number(item.authorizedCapital), 0),
      issuedCapital: data.reduce((sum, item) => sum + Number(item.issuedCapital), 0),
      };
      return totals;
    }
}
</script>
