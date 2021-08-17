<template>
  <div id="app">
    
    <div :class="{ 'order-form': true }">
      
      <div 
        :class="{ 'order-form__dish-builder-n-user-details': true, 
                    'order-form__dish-builder-n-user-details--disabled': isOrderFinished }"
      >
        
        <!-- Dish builder -->
        <div class="dish-builder 
                    order-form__dish-builder">
          
          <dish-components-menu 
            v-for="(optionsGroup,index) in dishModel"
            :key="optionsGroup.name"
            :menu-data="optionsGroup"
            class="dish-builder__options-menu" 
            :selected-options.sync="dishDetails[index].options"
          />
          
        </div><!-- / Dish builder -->
        
        
        <user-details 
          class="order-form__user-details"
          :user-data.sync="userData"
        />


        <generic-button
          :disabled="!ifEntireFormFilled"
          class="dish-components-menu__submit-button"
          text="Finish order"
          color="green"
          @click.native="isOrderFinished = true"
        />
        
      </div>
      
      <order-summary 
        v-if="isOrderFinished"
        class="order-form__order-summary" 
        :dish-model="dishModel"
        :dish-details="dishDetails"
        :user-data="userData"
      />
      
    </div>
    
  </div>
</template>

<script>
import pizzaModel from './pizza-model.json';

// Components
import DishComponentsMenu from './components/dish-components-menu';
import OrderSummary from './components/order-summary';
import UserDetails from './components/user-details';
import GenericButton from './components/generic-button.vue';

export default {
  name: 'PizzaMenu',
  
  components: {
    DishComponentsMenu, OrderSummary, UserDetails, GenericButton
  },
  
  data() {
    return {
      dishModel: [],

      isOrderFinished: false,
      
      dishDetails: [],
      userData: {
        name: '',
        address: '',
        phone: ''
      }
      
    }
  },
  
  mounted() {
    this.dishModel = JSON.parse(JSON.stringify(pizzaModel));
    
    this.initializeDishDetails();
    
  },
  
  updated() {
  },
  
  computed: {
    ifEntireFormFilled() {
      const isSizeSelected = this.dishDetails.find(item => item.name.toLowerCase() === 'size')?.options.length > 0;
      const is1OrMoreToppingsSelected = this.dishDetails.find(item => item.name.toLowerCase() === 'topping')?.options.length > 0;
      const isUserDataFilled = !!(this.userData.name && this.userData.address && this.userData.phone);

      return isSizeSelected && is1OrMoreToppingsSelected && isUserDataFilled;
    }
  },
  
  methods: {
    
    initializeDishDetails() {
      const dishDetailsDataStructure = [];
      
      for (let i = 0; i < this.dishModel.length; i++) {
        dishDetailsDataStructure[i] = { name: this.dishModel[i].name.toLowerCase(), options: [] };
      }
      
      this.dishDetails = dishDetailsDataStructure;
    },
  },
  
  watch: {
    dishDetails() {
     
    }
  }
  
}
</script>

<style scoped lang="scss">

#app {
  padding: 60px;
  font: 16px/22px Arial,sans-serif;
  color: rgb(53,73,94);
}

// ============================================================================
// Dish builder
.dish-builder {
  
  &__options-menu {
    margin-bottom: 40px;
  }
}


// ============================================================================
// Order form
.order-form {
  width: 1000px;
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  align-content: flex-start;
  align-items: flex-start;
  position: relative;
  
  &__dish-builder-n-user-details {
    width: calc(50% - 20px);
    
    &--disabled {
      opacity: .5;
      pointer-events: none;
    }
  }
  
    &__dish-builder {
      margin-bottom: 40px;
    }
  
    &__user-details {
      margin-bottom: 40px;
    }
  
  &__order-summary {
    width: calc(50% - 20px);
    position: sticky;
    inset: 20px 0 auto auto;
  }
}


</style>
