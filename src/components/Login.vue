<template>
    <div class="col-md-12">
      <div class="card card-container">
        <img
        id="profile-img"
        src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
        class="profile-img-card"
      />
        <form @submit.prevent="submitLoginForm">
          <div class="form-group">
            <label for="username">Username: </label>
            <input id="username" type="text" v-model="username" required />
          </div>
          <div class="form-group">
            <label for="password">Password: </label>
            <input id="password" type="password" v-model="password" required />
          </div>
          <br/>
          <div class="form-group">
          <button type="submit" class="btn btn-primary btn-block" :disabled="loading">
            <span
              v-show="loading"
              class="spinner-border spinner-border-sm"
            ></span>
            <span>Login</span>
          </button>
          <h6>Don't have an account? <a href="Register"> Register now</a></h6>
        </div>
        <div class="form-group">
          <div v-if="message" class="alert alert-danger" role="alert">
            {{ message }}
          </div>
        </div>
      </form>
    </div>
  </div>
  </template>
  
  <script>
  import axios from 'axios';
  //import { goToPage } from './Home.vue';
  
  export default {
    data() {
      return {
        username: '',
        password: '',
      }
    },
    methods: {
      async userRole() {
        const response = await axios.get('http://localhost:3000/users/me', {
          headers: {
            'Authorization': `Bearer ${localStorage.getItem('token')}`,
          }
        },);
        return response.data.role;
      },
      async login(username, password) {
        try {
          const response = await axios.post('http://localhost:3000/users/login', {username, password});
          localStorage.setItem('token', response.data.jwt);
          localStorage.setItem('username', username);
        } catch (error) {
          console.error(error);
        }
      },
      async submitLoginForm() {
        await this.login(this.username, this.password);
        this.$router.push({ path: '/data' });
      }
    }
  };
  </script>

<style scoped>
label {
  display: block;
  margin-top: 10px;
}

.card-container.card {
  max-width: 350px !important;
  padding: 40px 40px;
}

.card {
  background-color: #f7f7f7;
  padding: 20px 25px 30px;
  margin: 0 auto 25px;
  margin-top: 50px;
  -moz-border-radius: 2px;
  -webkit-border-radius: 2px;
  border-radius: 2px;
  -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
}

.profile-img-card {
  width: 96px;
  height: 96px;
  margin: 0 auto 10px;
  display: block;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
}

.error-feedback {
  color: red;
}
</style>
