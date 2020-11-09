<template>
  <div>
    <!-- Create new contact -->
    <form @submit.prevent="addContact">
      <h2 class="text-primary">Добавьте Контакт</h2>
      <input type="text" v-model="name" name="name" placeholder="Name" required/>
      <input type="text" v-model="phone" name="phone" placeholder="Phone"  required/>
      <div>
        <input
          type="submit"
          value='Add Contact'
          class="btn btn-primary btn-block"
        />
      </div>
    </form>
  </div>
</template>

<script>
import { v4 } from 'uuid';

export default {
  name: 'ContactForm',
  data(){
    return {
      name: '',
      phone:''
    }
  },
  methods: {
    // Add contact
    addContact(){
      const newContact = {
        id: v4(),
        name: this.name,
        phone: this.phone
      }
      // Send up to parent
      this.$emit('addContact', newContact)
      this.name = '';
      this.phone = '';
    }
  }
}
</script>
