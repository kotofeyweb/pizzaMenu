<template>
  <div class="user-details-form">
    
    <input 
      class="user-details-form__text-input"
      type="text" 
      placeholder="Name" 
      v-model="name"
    />
    <input 
      class="user-details-form__text-input"
      type="text" 
      placeholder="Address" 
      v-model="address"
    />
    <input 
      class="user-details-form__text-input"
      type="text" 
      placeholder="Phone" 
      v-model="phone"
    />
    
  </div>
</template>

<script>
export default {
  name: 'user-details',
  
  data() {
    return {
      name: '',
      address: '',
      phone: ''
    }
  },
  
  methods: {
    emitUserData() {
      this.$emit('update:user-data', { name: this.name, address: this.address, phone: this.phone });
    }
  },
  
  watch: {
    name() { this.emitUserData() },
    address() { this.emitUserData() },
    phone() { this.emitUserData() }
  }
}
</script>

<style scoped lang="scss">


// ============================================================================
// User details form
.user-details-form {
  padding: 30px;
  box-sizing: border-box;
  border-radius: 10px;
  background: rgba(53,73,94,.1);
  
  &__text-input {
    width: 100%;
    height: 30px;
    padding: 6px;
    margin-bottom: 15px;
    box-sizing: border-box;
    border: 1px rgba(53,73,94,.3) solid;
    font-size: 17px;
    line-height: 17px;
    
    &:last-child {
      margin-bottom: 0;
    }
  }
}



</style>