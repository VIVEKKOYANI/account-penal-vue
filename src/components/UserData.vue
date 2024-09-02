<template>
    <div class="card-row" v-if="allData.length > 0">
      <div class="card-column">
        <div class="card">
          <ul v-for="data in allData" :key="data.id">
            <li><img :src="data.image" :alt="data.name" /></li>
            <li v-if="!isEditing(data.id)">
              <strong>
                {{ data.name }}
                <span @click="startEdit(data.id)">
                  {{ isEditing(data.id) ? '✔' : '🖍' }}
                </span>
              </strong>
            </li>
            <li v-if="isEditing(data.id)">
              <input v-model="editableName" />
              <span @click="saveEdit(data.id)">
                {{ isEditing(data.id) ? '✔' : '🖍' }}
              </span>
            </li>
            <li>{{ data.about }}</li>
            <li>{{ data.email }}</li>
            <li>{{ data.password }}</li>
          </ul>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    props: ['allData'],
    setup(props) {
      const editingId = ref(null);
      const editableName = ref('');
  
      // Check if the current item is being edited
      const isEditing = (id) => {
        return editingId.value === id;
      }
  
      // Start editing mode for a specific item
      const startEdit = (id) => {
        editingId.value = id;
        // Find the item to edit and set its name to editableName
        const item = props.allData.find(data => data.id === id);
        if (item) {
          editableName.value = item.name;
        }
      }
  
      // Save the edited name
      const saveEdit = (id) => {
        const item = props.allData.find(data => data.id === id);
        if (item) {
          item.name = editableName.value; // Update the name in the data
        }
        editingId.value = null; // Exit editing mode
      }
  
      return { isEditing, startEdit, saveEdit, editableName };
    },
  }
  </script>
  
  <style scoped>
  .card-column {
    display: flex;
    justify-content: center;
    padding: 0 10px;
  }
  
  .card-row {
    margin-top: 1em;
    width: 100%;
  }
  
  .row:after {
    content: "";
    display: table;
    clear: both;
  }
  
  .card {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    text-align: center;
    background-color: #f1f1f1;
    display: flex;
    justify-content: center;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  @media screen and (max-width: 600px) {
    .card-column {
      width: 100%;
      display: block;
      margin-bottom: 20px;
    }
  }
  </style>
  