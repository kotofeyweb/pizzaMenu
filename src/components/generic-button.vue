<template>
  <button 
    :class="{ 'generic-button': true, 
                'generic-button--disabled': disabled,
                'generic-button--blue': color === 'blue',
                'generic-button--green': color === 'green' }"
  >
    {{ text }}
  </button>
</template>

<script>
export default {
  name: 'generic-button',

  props: {
    text: { type: String, default: 'Button' },
    color: { type: String, default: 'blue', validator: color => ['green','blue'].includes(color) },
    disabled: { type: Boolean, default: false }
  },

  data() {
    return {
    }
  }
}
</script>

<style scoped lang="scss">

// ============================================================================
.generic-button {
  height: 36px;
  padding: 0 30px;
  box-sizing: border-box;
  border: none;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-content: center;
  align-items: center;
  font-size: 17px;
  line-height: 17px;
  font-weight: bold;
  color: #fff;
  cursor: pointer;
  opacity: 1;
  transition: opacity .1s ease;
  
  &:hover {
    opacity: .8;
  }
  
  &--disabled {
    opacity: .4;
    pointer-events: none;
  }
  
  &--blue {
    background: rgba(100,150,255,1);
  }
  &--green {
    background: rgba(100,200,100,1);
  }
}

</style>