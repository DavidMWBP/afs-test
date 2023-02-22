<template>
    <div class="modal-backdrop">
      <div class="formModal">
        <form class="form" @submit.prevent="onSubmitForm">
            <div class="field">
                <label class="label">Security Class</label>
                <div class="control">
                <input v-model="formFields.name" class="input" type="text" placeholder="Enter Security Class">
                </div>
            </div>

            <div class="field">
                <label class="label">Authorized amount</label>
                <div class="control">
                <input v-model="formFields.authorizedAmount" class="input" type="number">
                </div>
            </div>

            <div class="field">
                <label class="label">Issued amount</label>
                <div class="control">
                <input v-model="formFields.issuedAmount" class="input" type="number">
                </div>
            </div>

            <div class="field">
                <label class="label">Authorized Capital</label>
                <div class="control">
                <input v-model="formFields.authorizedCapital" class="input" type="number">
                </div>
            </div>

            <div class="field">
                <label class="label">Issued capital</label>
                <div class="control">
                <input v-model="formFields.issuedCapital" class="input" type="number">
                </div>
            </div>

            <button type="submit">Submit</button>
        </form>
      </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue';

import Component from 'vue-class-component';
import { Prop } from 'vue-property-decorator';

import { TableData } from '@/types/types';

@Component({name: "Modal"})
    export default class Modal extends Vue {
        @Prop() tableData!: TableData[];
        formFields: TableData = {
          id: '',
          name: '',
          nominalValue: 0,
          authorizedAmount: 0,
          issuedAmount: 0,
          authorizedCapital: 0,
          issuedCapital: 0
        }

        // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
        async mounted() { 
          console.log(this.tableData)
        }

        onSubmitForm(): void {
        this.formFields.id = this.formFields.name;
        this.$emit("add-securityclass-item", this.formFields); // emit an event with the new item
        this.formFields = {
          id: "",
          name: "",
          nominalValue: 0,
          authorizedAmount: 0,
          issuedAmount: 0,
          authorizedCapital: 0,
          issuedCapital: 0,
        }; 
    }
  
    }
</script>

<style>
  .modal-backdrop {
    display: flex;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    justify-content: center;
    align-items: center;
  }

  .formModal {
    display: flex;
    padding: 20px;
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    flex-direction: column;
    width: 450px;
    align-self: center;
    justify-content: center;
  }

  .field {
    margin-bottom: 20px;
  }
</style>