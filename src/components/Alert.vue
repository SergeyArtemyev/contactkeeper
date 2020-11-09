<template>
  <div>
    <p>Are you sure?</p>
    <button class="btn btn-success btn-sm" @click="$emit('agreed', true)">Yes</button>
    <button class="btn btn-danger btn-sm" @click="$emit('agreed', false)">No</button>
  </div>
</template>

<script>
export default {
  name: 'Alert',
}
</script>

<style scoped>
  p {
    margin-right: 0.5rem;
  }
</style>