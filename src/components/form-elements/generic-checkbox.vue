<template>
  <div class="generic-checkbox">
    
    <input 
      class="generic-checkbox__checkbox-itself"
      type="checkbox"
      @click="handleInputClick"
      ref="input"
    />
    
    <div class="generic-checkbox__fake-checkbox">
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'generic-checkbox',
  
  data() {
    return {}
  },

  methods: {
    handleInputClick() {
      if (this.$refs.input.checked) {
        this.$emit('check')

      } else {
        this.$emit('uncheck')
      }
    }
  }
}
</script>

<style scoped lang="scss">

// ============================================================================
// Generic checkbox
.generic-checkbox {
  width: 16px;
  height: 16px;
  position: relative;
  
  &__checkbox-itself {
    width: 100%;
    height: 100%;
    position: absolute;
    inset: 0 auto auto 0;
    opacity: 0;
    visibility: hidden;
  }
  
  &__fake-checkbox {
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    border-radius: 4px;
    border: 2px rgba(53,73,94,.2) solid;
    position: absolute;
    inset: 0 auto auto 0;
    background: rgba(53,73,94,.1);
  }

  .generic-checkbox__checkbox-itself:checked + .generic-checkbox__fake-checkbox {

    &::before {
      content: '';
      width: 8px;
      height: 8px;
      border-radius: 2px;
      position: absolute;
      inset: 2px auto auto 2px;
      z-index: 4;
      background: rgba(53,73,94,.8);
      pointer-events: none;
    }
  }
}

</style>