<template>
  <div>
    <div class="form-group">
      <!-- Field -->
      <label><b>{{keyName}}</b>:</label>
      <input :class="{'disabled': !this.editedField}" class='input' type="text" v-model="fieldValue" disabled>
      <!-- Actions -->
      <!-- Edit/decline edit -->
      <i :class="{'edit':this.editedField}" class="fas fa-minus-circle text-danger" @click="editField"></i>
      <i :class="{'edit':this.editState, 'hide':this.editedField}" class="fas fa-pencil-alt" @click="editField"></i>
      <!-- Submit edit -->
      <button :class="{'edit':this.editedField}" class="btn btn-success btn-sm" @click="edit">submit</button>
      <!-- Delete field -->
      <i :class="{'edit':this.editState, 'hide': this.editedField}" class="fas fa-trash-alt text-danger" @click="deleteField"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContactField',
  props: ["keyName", "list", "contactInfo", 'updateStorage', "editState"],
  data(){
    return {
      fieldValue: '',
      editedField: false,
      changed: false,
      isDeleted: false,
      isEdited: false,
      changesArr: []
    }
  },
  methods: {
    // Submit edit
    edit(e){
      this.changed = true;
      this.isEdited = true;
      this.contactInfo[this.keyName] = this.fieldValue;
      this.editedField = !this.editedField;
      this.addChange();
      this.updateStorage(this.contactInfo);
      e.target.previousElementSibling.previousElementSibling.disabled = !e.target.previousElementSibling.previousElementSibling.disabled;
    },
    // Enable edit field
    editField(){
      this.changed = true;
      this.editedField = !this.editedField;
      if(!this.editedField && !confirm()){
        this.editedField = !this.editedField;
      } else {
        document.querySelector('.input').disabled = !document.querySelector('.input').disabled;
      }
    },
    // Delete field
    deleteField(e){
      if(confirm()){
        this.isDeleted = true;
        this.changed = true;
        this.addChange();
        e.target.parentElement.remove();
        delete this.contactInfo[this.keyName];
        this.updateStorage(this.contactInfo);
      }
    },
    // Send up changes
     addChange(){
      let change = {};
      change[this.keyName] = this.fieldValue;
      this.changesArr.push(change)
      this.$emit('loadChanges', this.changesArr, this.changed, this.isEdited, this.isDeleted)
     }
  },
  // Load values
  created(){
    this.fieldValue = this.list;
  }
}
</script>

<style scoped>
  .form-group {
    display: flex;
    align-items: center;
    margin: 0;
    gap: 0.3rem;
  }
  .disabled {
    border: 0
  }
  .hide {
    display: none !important;
  }
  i, button {
    display: none;
  }
  .edit {
    display: block;
  }
  i {
    cursor: pointer;
  }
  
</style>