<template>
    <div class="container">
        <div class="form-control">
            <ProfilePicture :email="email" :image="image" @update:image="image = $event" />
        </div>
        <div class="row">
            <div class="col-25">
                <label for="name">Name</label>
            </div>
            <div class="col-75">
                <input type="text" v-model="name" @input="validateName('firstName')"/>
                <span v-if="errors.name" class="error">{{ errors.name }}</span>
            </div>
        </div>
        <div class="row">
            <div class="col-25">
                <label for="about">About</label>
            </div>
            <div class="col-75">
                <textarea v-model="about"></textarea>
            </div>
        </div>
        <div class="row">
            <div class="col-25">
                <label for="email">Email</label>
            </div>
            <div class="col-75">
                <input type="email" v-model="email" />
            </div>
        </div>
        <div class="row">
            <div class="col-25">
                <label for="password">Password</label>
            </div>
            <div class="col-75">
                <input type="password" v-model="password" />
            </div>
        </div>
        <div class="row">
            <button class="addBtn" @click="addNewData">Add</button>
        </div>
    </div>
    <UserData :allData="allData" />
</template>

<script>
import { ref } from 'vue';
import ProfilePicture from './ProfilePicture.vue';
import UserData from './UserData.vue';

export default {
    components: {
        ProfilePicture,
        UserData
    },
    setup() {

        const image = ref(null);
        const name = ref(null);
        const about = ref(null);
        const email = ref(null);
        const password = ref(null);
        const allData = ref([]);
        const errors = ref({});

        const validateName = (field) => {
                if (field === 'name' && (name.value.length > 20 || !name.value)) {
                    errors.value.name = 'First name is required and must be under 20 characters.';
                } else {
                    errors.value[field] = '';
                }
            }

        const addNewData = () => {
          validateName('name');
          if (!Object.values(errors.value).some(error => error)) {
            allData.value.push({
                id: new Date().toISOString(),
                image: image.value,
                name: name.value,
                about: about.value,
                email: email.value,
                password: password.value
            });

            // Reset the form fields
            image.value = '';
            name.value = '';
            about.value = '';
            email.value = '';
            password.value = '';
          }
        };

        return {
            name,
            image,
            email,
            password,
            about,
            allData,
            addNewData,
            validateName,
            errors
        };
    },
};
</script>

<style scoped>
input,
textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  resize: vertical;
}

.form-control {
  margin: 0.5rem 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

label {
  padding: 12px 12px 12px 0;
  display: inline-block;
}
.addBtn {
  background-color: #04AA6D;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  float: right;
  margin-top: 10px;
}

.addBtn:hover {
  background-color: #45a049;
}

.container {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}

.col-25 {
  float: left;
  width: 25%;
  margin-top: 6px;
}

.col-75 {
  float: left;
  width: 75%;
  margin-top: 6px;
}

.row:after {
  content: "";
  display: table;
  clear: both;
}


/* Responsive layout - when the screen is less than 600px wide, make the two columns stack on top of each other instead of next to each other */
@media screen and (max-width: 600px) {
  .col-25,
  .col-75,
  input[type=submit] {
    width: 100%;
    margin-top: 0;
  }
}
</style>