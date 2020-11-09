<template>
  <div class="container">
    <h1 class="text-center">Контакт</h1>
    <div class="grid-2-3 card bg-light">
      <div class="avatar">
        <img src="../assets/img/user.jpg" alt="avatar">
        <h3 class="text-primary text-center">{{contactInfo.name}}</h3>
      </div>
      <div class="info">
        <!-- Individual field, hide field with id -->
        <div class='info-item' :class="keyName" :key="index" v-for="(list, keyName, index) in contactInfo">
          <ContactField
          v-if="keyName !== 'id'"
          :key="upd"
          v-on:loadChanges="loadChanges" 
          :contactInfo="contactInfo" 
          :updateStorage="updateStorage" 
          :keyName="keyName" 
          :list="list"
          :editState="editState"
          />
        </div>
        <!-- Add new field -->
        <form class="new-fields-form" :class="{'show-form': this.showForm}" @submit.prevent="edit">
          <input v-model="editContact.newName" type="text" name='newName' placeholder="type" required>
          <input v-model="editContact.newValue" type="text" name='newValue' placeholder="value" required>
          <input type="submit" class="btn btn-success btn-sm" value="Submit">
        </form>
        <button :class="{'edit-form': this.editState, 'btn-danger': this.showForm}" class="btn btn-success btn-sm hide my-1" @click="showFields">{{this.showForm ? 'Cancel' : 'Add New Field' }}</button>
        <!-- Change state button -->
        <button :class="{'btn-danger':this.editState, 'hide':this.showForm}" class="btn btn-dark btn-sm" @click="changeState">{{this.editState ? 'Cancel' : 'Edit'}}</button>
        <!-- Back to contacts -->
        <router-link class="btn btn-dark btn-sm" :class="{'hide': this.editState}" to='/'>Back To Contacts</router-link>
        <!-- Step back -->
        <button :class="{'hide': !this.hasChanged}" class="btn btn-danger btn-sm" @click="cancelChange">1 step back</button>
      </div>
    </div>
  </div>
</template>

<script>
import ContactField from '../components/ContactField';

export default {
  name: 'Contact',
  components: {
    ContactField,
  },
  data() {
    return {
      contactInfo: '',
      editContact: {
        newName:'',
        newValue:'',
      },
      editState: false,
      showForm: false,
      editedField: false,
      editValue: '',
      hasChanged: false,
      initValues: '',
      deleted: false,
      edited: false,
      changes: [],
      upd: 0
    }
  },
  methods: {
    // Open edit state
    changeState(){
      this.editState = !this.editState;
      this.initValues = {...this.contactInfo};
      this.hasChanged = false;
    },
    // Submit edit
    edit(){
      const { newName, newValue } = this.editContact;
      this.contactInfo = {...this.contactInfo, [newName]: newValue};
      this.changes.push({[newName]: newValue})
      this.updateStorage(this.contactInfo)
      this.editContact.newName = '';
      this.editContact.newValue = '';
      this.showForm = false;
      this.editState = false;
      const form = document.querySelector('.new-fields-form');
      form.classList.remove('edit-form');
    },
    // Update localStorage function
    updateStorage(updContact){
      const contacts = JSON.parse(localStorage.getItem('contacts'));
      const contact = contacts.filter(contact => contact.id === this.$route.params.id)[0];
      const index = contacts.indexOf(contact);
      contacts[index] = updContact;
      localStorage.setItem('contacts', JSON.stringify(contacts))
      this.forseRender()

    },
    // Catch changes from ContactField component
    loadChanges(arr, bool, ed, del){
      this.hasChanged = bool;
      this.deleted = del;
      this.edited = ed;
      arr.forEach(arrEl => this.changes = [...this.changes, arrEl])
    },
    // Forse render for components
    // Была проблема, что компонент не обновлялся, хотя стейт был изменен
    forseRender(){
      this.upd += 1;
    },
    cancelChange(){
      // If it was delete action uses change array
      if(this.deleted){
        const change = this.changes[this.changes.length - 1];
        for (const [key, value] of Object.entries(change)){
          this.contactInfo[key] = value;
        }
        this.contactInfo = {...this.contactInfo}
        this.updateStorage(this.contactInfo)
        this.forseRender()
      } else if(this.edited){
        // If it was edit uses initial values
        for (const [key, value] of Object.entries(this.initValues)){
          this.contactInfo[key] = value;
        }
        this.contactInfo = {...this.contactInfo}
        this.updateStorage(this.contactInfo)
        this.forseRender()
      }
      this.hasChanged = !this.hasChanged
    },
    // Open new field form
    showFields(){
      this.showForm = !this.showForm;
      this.editContact.newName = '';
      this.editContact.newValue = '';
    }
  },
  created(){
    // Get requested contact
    const contacts = JSON.parse(localStorage.getItem('contacts'));
    const reqContact = contacts.find(contact => contact.id === this.$route.params.id);
    this.contactInfo =reqContact;
  }
}
</script>

<style scoped>
  .container {
    max-width: 900px;
  }
  .avatar {
    width: 200px;
    margin: 0 auto;
  }
  .hide, .alert {
    display: none;
  }
  .edit-form, .show-alert {
    display: block;
  }
  .show-form {
    display: flex !important;
  }

  .new-fields-form {
    display: none;
    gap: 0.5rem;
  }
  input[name = 'newName'] {
    width: 100px;
  }
</style>