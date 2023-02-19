<template>
    <div class="col-md-12">
      <div class="card card-container">
          <img
          id="profile-img"
          src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
          class="profile-img-card"
        />
          
          <form @submit.prevent="submitRegisterForm">
            <div class="form-group">
              <label for="username">Username: </label>
              <input id="username" type="text" v-model="username" required />
            </div>
            <div class="form-group">
              <label for="password">Password: </label>
              <input id="password" type="password" v-model="password" required />
            </div>
            
            <p class="error-message" v-if="errorMessage">{{ errorMessage }}</p>
            <br/>
            <button type="submit"  class="btn btn-primary btn-block" :disabled="loading">
              <span
                v-show="loading"
                class="spinner-border spinner-border-sm"
              ></span>
              Register
            </button>
            <h6>Already have an account? <a href="Login"> log in now</a></h6>
          </form>
        </div>
      </div>
  </template>
  
  <script>
  import axios from 'axios';
  import Login from './Login.vue';
  export default {
    data() {
      return {
        username: '',
        password: '',
        errorMessage: '',
      }
    },
    methods: {
      async submitRegisterForm() {
        try {
          const response = await axios.post('http://localhost:3000/users/register', {
            username: this.username,
            password: this.password,
          });
          //await Login.login(this.username, this.password);
  
          try {
            const response = await axios.post('http://localhost:3000/users/login', {
              username: this.username,
              password: this.password,
            });
            localStorage.setItem('token', response.data.jwt);
            localStorage.setItem('username', this.username);
          } catch (error) {
            console.error(error);
          }
  
          this.$router.push({ path: '/' });
        } catch (error) {
          if (error.response.status === 400) {
            this.errorMessage = "Username already taken !";
          } else {
            this.errorMessage = "An error occurred, please try again later";
          }
        }
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
.error-message {
      color: red;
    }
</style>