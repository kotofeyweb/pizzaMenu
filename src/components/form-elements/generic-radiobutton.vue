<template>
  <div class="generic-radiobutton">

    <input
      class="generic-radiobutton__radiobutton-itself"
      type="radio"
      :name="name || ''"
      @click="handleInputClick"
      ref="input"
    />

    <div class="generic-radiobutton__fake-radiobutton">
    </div>

  </div>
</template>

<script>
export default {
  name: 'generic-radiobutton',
  
  props: { 
    name: { type: String }
  },

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
// Generic radiobutton
.generic-radiobutton {
  width: 16px;
  height: 16px;
  position: relative;

  &__radiobutton-itself {
    width: 100%;
    height: 100%;
    position: absolute;
    inset: 0 auto auto 0;
    z-index: 3;
    opacity: 0;
  }

  &__fake-radiobutton {
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    border-radius: 100px;
    border: 2px rgba(53,73,94,.2) solid;
    position: absolute;
    inset: 0 auto auto 0;
    background: rgba(53,73,94,.1);
  }
  
  .generic-radiobutton__radiobutton-itself:checked + .generic-radiobutton__fake-radiobutton {
    
    &::before {
      content: '';
      width: 8px;
      height: 8px;
      border-radius: 100px;
      position: absolute;
      inset: 2px auto auto 2px;
      z-index: 4;
      background: rgba(53,73,94,.8);
      pointer-events: none;
    }
  }
}

</style>