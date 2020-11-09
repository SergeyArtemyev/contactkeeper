<template>
  <div>
    <div class="grid-2 container">
      <ContactForm v-on:addContact="addContact"/>
      <div>
        <ContactFilter v-on:filter="filterContacts"/>
        <Alert class='alert' :class="{'show-alert': this.alert}" v-on:agreed="agreed"/>
        <Contacts :contacts="contacts" v-on:delete-contact="showAlert"/>
      </div>
    </div>
  </div>
</template>

<script>
import ContactFilter from '../components/ContactFilter';
import ContactForm from '../components/ContactForm';
import Contacts from '../components/Contacts';
import Alert from '../components/Alert';


export default {
  name: 'Home',
  components: {
    ContactForm,
    Contacts,
    ContactFilter,
    Alert
  },
  data(){
    return {
      contacts: [],
      alert: false,
      agree: false,
      id: ''
    }
  },
  methods: {
    // Add new contact
    addContact(newContact){
      this.contacts = [newContact, ...this.contacts]
      localStorage.setItem('contacts', JSON.stringify(this.contacts));
    },
    // Catch id and show alert
    showAlert(id){
      this.alert = true;
      this.id = id;
      
    },
    // Catch desecion and delete contact   
    agreed(bool){
      this.agree = bool;
      if(this.agree){
        this.deleteContact(this.id)
      }
      this.alert = false;
      this.id = '';
    },
    // Delete Contact
    deleteContact(id){
      this.contacts = this.contacts.filter(contact=> contact.id !== id);
      localStorage.setItem('contacts', JSON.stringify(this.contacts));
    },
    // Filter contacts
    filterContacts(text){
      this.contacts = this.contacts.filter(contact => {
        const regex = new RegExp(text, "gi")
        return contact.name.match(regex) || contact.phone.match(regex)
      })
    }
  }, 
  // Load all contacts 
  created(){
    if(!localStorage.getItem('contacts')){
      this.contacts = []
    } else {
      this.contacts = JSON.parse(localStorage.getItem('contacts'))
    }
  }
}
</script>

<style scoped>
  .alert {
    display: none;
  }

  .show-alert {
    display: flex;
  }
</style>
