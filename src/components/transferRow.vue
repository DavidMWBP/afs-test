<template>
  <div class="transfer-row">
    <div :class="`transfer-row__side-color${transferState}`"></div>
    <div class="transfer-row__content">
      <div class="left-content">
        <p class="amount">${{ transfer.amount }}</p>
        <div class="date-section">
          <p class="date">DATE</p>
          <p>{{ transfer.recordDate }}</p>
        </div>
      </div>
      <div class="vertical-line"></div>
      <div class="right-content">
        <div class="right-content__main-details">
          <div :class="`right-content__main-details__dottedLine${transferState}`">
            <span class="right-content__main-details__dottedLine__circle">⚬</span>
            <span :class="`right-content__main-details__dottedLine__dots${transferState}`"></span>
            <span class="right-content__main-details__dottedLine__circle">⚬</span>
          </div>
          <div class="right-content__main-details">
            <p class="right-content__main-details__item">{{ transfer.type }}</p>
            <span class="right-content__main-details__horizontal-line"></span>
            <p class="right-content__main-details__item" v-if="!transfer.forgottenProperty">Some Address</p>
            <p class="right-content__main-details__item" v-if="transfer.forgottenProperty">{{ transfer.forgottenProperty }}</p>
          </div>
        </div>
        <div class="extra-items">
          <div class="extra-items__item">
            <p>STATE</p>
            <p class="extra-items__item__subtitle">{{ transfer.state }}</p>
          </div>     
          <div class="extra-items__item">
            <p>PLEDGE</p>
            <p class="extra-items__item__subtitle">${{ randomPledgeNumber }}</p>
          </div>     
          <div class="extra-items__item">
            <p>WEIGHT</p>
            <p class="extra-items__item__subtitle">light</p>
          </div>      
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

import {
  Component,
  Prop,
} from 'vue-property-decorator';

import { Transaction } from '@/types/types';

@Component({
    name: "TransferRow",
  })
  export default class TransferRow extends Vue {
    @Prop({ required: true }) transfer!: Transaction;

    get transferState(): string {
      return this.transfer.state ? `--${this.transfer.state.toLowerCase()}` : "";
    }

    get randomPledgeNumber(): number {
      return Math.floor(Math.random() * 1000);
    }
  }
</script>

<style lang="scss">
  $side-color-width: 8px;
  $color-grey: #757677;
  $color-light-grey: #cacaca;
  $color-green: #51a43e;
  $color-red: #eb5350;
  $color-purple: #be63c7;
  $color-orange: #f9a918;

  %side-color {
    position: absolute;
    width: $side-color-width;
    height: 100%;
    background-color: $color-orange;
  }

  %dotted-line {
    display: flex;
    color: $color-orange;
    font-weight: 600;
    height: 100%;
    font-size: 20px;
    align-items: center;
    flex-direction: column;
  }

  %dots {
    height: 100%;
    margin-bottom: -2px;
    border-left: 2px dotted $color-orange;
  }

  .transfer-row {
    position: relative;
    width: 100%;
    max-width: 450px;
    height: 200px;
    background-color: #fdfdfe;
    border-radius: 10px;
    margin: 1rem;
    overflow: hidden;

    .transfer-row__side-color {
      @extend %side-color;

      &--new {
        @extend %side-color;
        background-color: $color-green;
      }

      &--old {
        @extend %side-color;
        background-color: $color-red;
      }

      &--published {
        @extend %side-color;
        background-color: $color-purple;
      }

      &--modified {
        @extend %side-color;
        background-color: $color-orange;
      }
    }

    .transfer-row__content {
      display: flex;
      height: 100%;
      margin-left: $side-color-width;
      font-size: 14px;
      word-break: break-all;

      .left-content {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: flex-start;
        flex-basis: 140px;
        padding: 20px;
        color: $color-grey;

        .amount {
          font-size: 1.5rem;
        }
      }
  
      .right-content {
        display: flex;
        flex: 1 1;
        flex-direction: column;
        justify-content: space-between;
        padding: 20px;

        .right-content__main-details {
          display: flex;
          column-gap: 10px;
          height: 30%;
          line-height: 16px;

          &__dottedLine {
            @extend %dotted-line;
          
            &--new {
              @extend %dotted-line;
              color: $color-green;
            }

            &--old {
              @extend %dotted-line;
              color: $color-red;
            }

            &--published {
              @extend %dotted-line;
              color: $color-purple;
            }

            &--modified {
              @extend %dotted-line;
              color: $color-orange;
            }
          }

          .right-content__main-details__dottedLine__circle {
            line-height: 14px;
          }

          .right-content__main-details__dottedLine__dots {
            @extend %dots;
          
            &--new {
              @extend %dots;
              border-left: 2px dotted $color-green;
            }

            &--old {
              @extend %dots;
              border-left: 2px dotted $color-red;
              
            }

            &--published {
              @extend %dots;
              border-left: 2px dotted $color-purple;
            }

            &--modified {
              @extend %dots;
              border-left: 2px dotted $color-orange;
            }
          }

          .right-content__main-details {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;

            .right-content__main-details__item {
              place-self: flex-start;
            }

            .right-content__main-details__horizontal-line {
              border-bottom: 1px solid $color-light-grey;
              height: 1px;
              width: 100%;
            }
          }
        }

        .extra-items {
          display: flex;
          flex-direction: row;
          justify-content: space-between;
          color: $color-light-grey;

          .extra-items__item {
            display: flex;
            flex-direction: column;
            align-items: flex-start;

            &__subtitle {
              font-weight: 600;
            }
          }

          p {
            margin: 0;
          }
        }
      }

      .vertical-line {
        align-self: center;
        border-left: 2px solid #f0f0f0;
        height: calc(100% - 20px);
      }
    }
  }

  .date-section {
    display: flex;
    flex-direction: column;
    align-items: flex-start;

    p {
      margin: 0;
    }

    .date {
      color: $color-light-grey;
    }
  }
</style>
