<template>
  <div class="dish-components-menu">
    
    <!-- Main title -->
    <div class="main-title 
                dish-components-menu__main-title">
      <div 
        class="main-title__icon"
        v-html="{'size': icons.measuringTapeIcon, 'topping': icons.pizzaToppingIcon}
                        [menuData.name.toLowerCase()]"
      ></div>
      
      {{ 'Select ' + menuData.name }}
    </div><!-- / Main title -->
    
    <!-- Options list -->
    <ul :class="{ 'options-list': true,
                    'dish-components-menu__options-list': true }">
      <!-- Single option -->
      <li
        v-for="option in menuData.options"
        :key="option.name"
        class="single-option 
                options-list__single-option"
      >
        <label class="single-option__label">
          <generic-radiobutton
            v-if="menuData.maxSelected === 1"
            class="single-option__checkbox"
            :name="menuData.name"
            @check="selectOption(option.name)"
            @uncheck="unselectOption(option.name)"
          />
          <generic-checkbox
            v-else
            class="single-option__checkbox"
            @check="selectOption(option.name)"
            @uncheck="unselectOption(option.name)"
          />
          
          <div class="single-option__name">
            {{ option.name }}
          </div>
          <div class="single-option__price">
            {{ option.price + '$' }}
          </div>
        </label>
      </li><!-- / Single option -->
    </ul><!-- / Options list -->
    
    <!--<generic-button -->
    <!--  class="dish-components-menu__submit-button" -->
    <!--  text="Continue"-->
    <!--/>-->
    
  </div>
</template>

<script>
// Components
import GenericButton from './generic-button.vue';
import GenericCheckbox from './form-elements/generic-checkbox.vue';
import GenericRadiobutton from './form-elements/generic-radiobutton.vue';

// Icons
import pizzaToppingIcon from '!!raw-loader!../assets/icons/pizza-topping.svg';
import measuringTapeIcon from '!!raw-loader!../assets/icons/measuring-tape.svg';

export default {
  name: 'dish-components-menu',
  
  props: {
    menuData: { type: Object, required: true }
  },
  
  components: {
    GenericButton, GenericCheckbox, GenericRadiobutton
  },
  
  data() {
    return {
      selectedOptions: [],
      
      icons: {
        pizzaToppingIcon, measuringTapeIcon
      }
    }
  },
  
  updated() {
    console.log('%c this.selectedOptions: ','background:greenyellow;color:black;', this.selectedOptions);
  },
  
  methods: {
    selectOption(optionName) {
      if (this.menuData.maxSelected === 1) {
        this.selectedOptions = [optionName];
      } else {
        this.selectedOptions = [...this.selectedOptions, optionName];
      }
    },
    
    unselectOption(optionName) {
      this.selectedOptions = this.selectedOptions.filter(item => item !== optionName);
    }
  },
  
  watch: {
    selectedOptions() {
      this.$emit('update:selected-options', this.selectedOptions);
    }
  }
}
</script>

<style scoped lang="scss">


// ============================================================================
// Main title
.main-title {
  height: 50px;
  padding: 15px;
  box-sizing: border-box;
  border-radius: 5px;
  display: flex;
  align-content: center;
  align-items: center;
  font-size: 18px;
  line-height: 22px;
  font-weight: bold;
  background: rgba(53,73,94,.1);
  user-select: none;
  cursor: pointer;

  &__icon {
    width: 26px;
    height: 26px;
    margin-right: 6px;

    ::v-deep svg {
      width: 100%;
      height: 100%;
      fill: rgba(53,73,94,1);
    }
  }
}



// ============================================================================
// Single option
.single-option {
  border: 2px rgba(53,73,94,.1) solid;
  display: flex;
  justify-content: space-between;
  align-content: center;
  align-items: center;
  user-select: none;
  
  &__label {
    width: 100%;
    height: 100%;
    padding: 7px 12px;
    box-sizing: border-box;
    display: flex;
    justify-content: space-between;
    align-content: center;
    align-items: center;
    cursor: pointer;
  }
  
    &__checkbox {
      margin-right: 7px;
    }
    
    &__name {
      flex-grow: 1;
    }
    
    &__price {
      width: 30px;
      font-style: italic;
      color: rgba(80,180,80,1);
    }
}



// ============================================================================
// Options list
.options-list {
  padding: 0;
  margin: 0;
  position: relative;
  overflow: hidden;
  list-style: none;
  
  &__single-option {
    border-top-width: 0;
    
    &:first-child { border-top-width: 2px; border-radius: 5px 5px 0 0; }
    &:last-child { border-radius: 0 0 5px 5px; }
  }
}



// ============================================================================
// Dish components menu
.dish-components-menu {
  
  &__main-title {
    margin-bottom: 10px;
  }
  
  &__options-list {
    max-height: 1000px;
    margin-bottom: 10px;

    &--collapsed {
      max-height: 0;
    }
  }
  
  &__submit-button {}
}


</style>