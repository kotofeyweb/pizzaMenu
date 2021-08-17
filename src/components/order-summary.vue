<template>
  <div class="order-summary">
    
    <!-- Block placeholder -->
    <div 
      v-if="isDishDetailsEmpty"
      class="block-placeholder 
              order-summary__block-placeholder"
    >
      
      <div 
        class="block-placeholder__icon" 
        v-html="icons.pizzaIcon"
      />
      
      <div class="block-placeholder__title">
        Order summary
      </div>
    </div><!-- / Block placeholder -->
    
    <div
      v-else
      class="order-summary__summary"
    >
      <ul class="user-data 
                  order-summary__user-data">
        <li class="user-data__section">
          <div class="user-data__title">Name: </div>
          <div class="user-data__value">{{ userData.name }}</div>
        </li>
        <li class="user-data__section">
          <div class="user-data__title">Address: </div>
          <div class="user-data__value">{{ userData.address }}</div>
        </li>
        <li class="user-data__section">
          <div class="user-data__title">Phone: </div>
          <div class="user-data__value">{{ userData.phone }}</div>
        </li>
      </ul>
      <div class="total-info 
                  order-summary__total-info">
        <div class="total-info__section">
          <div class="total-info__title">Subtotal: </div>
          <div class="total-info__value">${{ Math.round((totalSum - discount) * 10) / 10 }}</div>
        </div>
        <div class="total-info__section">
          <div class="total-info__title">Discount: </div>
          <div class="total-info__value">${{ Math.round((discount) * 10) / 10 }}</div>
        </div>
        <div class="total-info__section">
          <div class="total-info__title">VAT: </div>
          <div class="total-info__value">${{ Math.round((totalSum / 100 * 15) * 10) / 10 }}</div>
        </div>
        
        <div class="total-info__section">
          <div class="total-info__title 
                      total-info__title--big">Total: </div>
          <div class="total-info__value 
                      total-info__value--big">${{ Math.round((totalSumWithVat - discount) * 10) / 10 }}</div>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
// Icons
import pizzaIcon from '!!raw-loader!../assets/icons/pizza.svg';

export default {
  name: 'order-summary',
  
  props: { 
    dishDetails: { type: Array, required: true },
    dishModel: { type: Array, required: true },
    userData: { type: Object, required: true }
  },
  
  components: {
    pizzaIcon
  },
  
  data() {
    return {
      icons: {
        pizzaIcon
      }
    }
  },
  
  computed: {
    isDishDetailsEmpty() {
      return this.dishDetails.reduce((acc,item) => {
        if (item.options.length === 0) return acc
        else return [...acc,1]
      }, []).length === 0;
    },

    totalSum() {
      const allModelOptions = this.dishModel.reduce((acc,item) => [...acc, ...item.options], []);
      const allSelectedOptions = this.dishDetails.reduce((acc,item) => [...acc, ...item.options], []);
      
      const totalPrice = allSelectedOptions.reduce((acc,item) => {
        const itemPrice = allModelOptions.find(innerItem => innerItem.name === item).price;
        
        return acc + itemPrice;
      }, 0);
      
      return totalPrice;
    },
    
    discount() {
      let discount = 0;
      
      // Calculate discount
      const allModelValuableToppings = this.dishModel
          .find(item => item.name.toLowerCase() === 'topping').options
          .filter(item => item.price !== 0)
          .sort((a,b) => a.price > b.price ? 1 : -1);
      const allSelectedValuableToppings = this.dishDetails
          .find(item => item.name.toLowerCase() === 'topping').options
          .filter(item => allModelValuableToppings.findIndex(itm => itm.name === item) !== -1);

      if (allSelectedValuableToppings.length >= 3) {
        const howManyToppingsForFree = Math.floor(allSelectedValuableToppings.length / 3);
        discount = allModelValuableToppings.slice(0,howManyToppingsForFree).reduce((acc,item) => acc + item.price, 0);
      }
      
      return discount;
    },
    
    totalSumWithVat() {
      return this.totalSum + (this.totalSum / 100 * 15);
    }
  }
}
</script>

<style scoped lang="scss">


// ============================================================================
// Block placeholder
.block-placeholder {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  
  &__icon {
    width: 150px;
    height: 150px;
    margin-bottom: 25px;
    
    ::v-deep svg {
      width: 100%;
      height: 100%;
      fill: rgba(180,180,100,.2)
    }
  }
  
  &__title {
    font-size: 30px;
    color: rgba(180,180,100,.55);
  }
}


// ============================================================================
// Order summary
.order-summary {
  padding: 40px 40px 60px;
  box-sizing: border-box;
  border-radius: 5px 5px 0 0;
  background: repeat-x 0 100%/20px 10px rgba(200,200,0,.2);
  background-image: url(../assets/triangle.png);
  
  &__block-placeholder {}
  
  &__summary {}

    .user-data {
      padding: 0;
      margin: 0;
      list-style: none;
      border-bottom: 1px rgba(53,73,94,.2) solid;
    
      &__section {
        display: flex;
        align-content: center;
        align-items: center;
      }
    
      &__title {
        margin-right: 10px;
        font-weight: bold;
      }
      &__value {}
    }
    &__user-data {
      padding-bottom: 20px !important;
      margin-bottom: 20px !important;
    }
  
    &__total-info {}
    .total-info {
      
      &__section {
        margin-bottom: 10px;
        display: flex;
        align-content: center;
        align-items: center;
        
        &:last-child {
          margin-bottom: 0;
        }
      }
      
        &__title {
          font-size: 22px;
          
          &--big {
            font-size: 26px;
            font-weight: bold;
          }
        }
        
        &__value {
          margin-left: 7px;
          font-size: 22px;
          color: rgba(100,180,100,1);
          
          &--big {
            font-size: 30px;
            font-weight: bold;
          }
        }
    }
}


</style>