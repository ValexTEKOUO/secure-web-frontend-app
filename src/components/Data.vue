<template>
<div class="card card-container">
      <strong>FILMS</strong>
      <div>
        <div v-for="film in currentFilms" :key="film._id">
          <button @click="goToFilmDetails(film._id)">{{ film.filmName }}</button>
        </div>
        <br />
        <div>
          <button type="submit" class="btn btn-success" @click="previousPage">Previous</button>

          <button type="submit" class="btn btn-success" @click="nextPage">Next</button>

            <button type="submit" class="btn btn-primary btn-block" @click="createFilm">Add Film</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  export default {
    data() {
      return {
        token: localStorage.getItem('token'),
        films: [],
        currentPage: 0,
        itemsPerPage: 8,
        role: '',
      };
    },
    computed: {
      currentFilms() {
        const start = (this.currentPage - 1) * this.itemsPerPage;
        const end = start + this.itemsPerPage;
        return this.films.slice(start, end);
      },
    },
    async mounted() {
      await this.fetchFilms();
      this.role = await this.userRole();
    },
    methods: {
      async userRole() {
        try {
          const response = await axios.get('http://localhost:3000/users/me', {
            headers: {
              'Authorization': `Bearer ${this.token}`,
            }
          },);
          return response.data.role;
        } catch (error) {
          return error.response;
        }
      },
      async fetchFilms() {
        const { data } = await axios.get('http://localhost:3000/locations', {
          headers: {
            'Authorization': `Bearer ${this.token}`,
          }});
        this.films = data;
      },
      goToFilmDetails(filmId) {
        this.$router.push(`/filmDetails/${filmId}`);
      },
      createFilm() {
        this.$router.push('/createFilm');
      },
      previousPage() {
        if (this.currentPage > 1) {
          this.currentPage--;
        }
      },
      nextPage() {
        if (this.currentPage < Math.ceil(this.films.length / this.itemsPerPage)) {
          this.currentPage++;
        }
      },
    },
  };
  </script>