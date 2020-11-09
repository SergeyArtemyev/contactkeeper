<template>
  <div>
    <div :key="contact.id" v-for="contact in contacts">
      <ContactItem :contact="contact" v-on:delete-contact="$emit('delete-contact', contact.id)"/>
    </div>
  </div>
</template>

<script>
import ContactItem from './ContactItem'

export default {
  name: 'Contacts',
  components: {
    ContactItem
  },
  props: ["contacts"]
}
</script>
