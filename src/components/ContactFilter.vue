<template>
  <div>
    <form>
      <input type="text" v-model="text" placeholder="Filter Contacts..." @input="filter">
    </form>
  </div>
</template>

<script>
export default {
  name: 'ContactFilter',
  data(){
    return {
      text: ''
    }
  }, 
  methods: {
    // Этот метод работает наполовину, я не смог разобраться как сделать onchange
    // поэтому фильтр работает только при вводе
    filter(){
     this.$emit('filter', this.text)
    }
  }
}
</script>
