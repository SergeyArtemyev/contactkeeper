<template>
  <div class="card bg-light">
    <h3 class="text-primary text-left">{{contact.name}}</h3>
    <ul class="list">
      <li class="fas fa-phone"></li> {{contact.phone}}
    </ul>
    <p>
      <!-- Back to Contacts -->
      <router-link :to="'/' + contact.id" class="btn btn-dark btn-sm">
        View
      </router-link>
      <!-- Delete Contact -->
      <button class="btn btn-danger btn-sm" @click="$emit('delete-contact', contact.id)">
        Delete
      </button>
    </p>
  </div>
</template>

<script>

export default {
  name: 'ContactItem',
  props: ["contact"]
}
</script>
