
<template>


  <div class="container">
    <div class="perfile">
       <h1>GITHUB PROFILES</h1>
    </div>
    <div class="input">

        <input v-model="user" placeholder="Username" type="text">
        <button @click.prevent="getUser">Buscar</button>

    </div>

   <div v-if="profile" class="profile">
     <div class="profileimg">
      <img v-if="profile.avatar_url" :src="profile.avatar_url" alt="" />
      <p>  {{ profile.name }} </p>

     </div>
     <div class="profileavatar" v-if="profile.followers && profile.public_repos">

        <p><img src="../assets/users-solid.svg" alt="followers profile"> {{ profile.followers }}</p>
        <p><img src="../assets/book-solid.svg" alt="followers profile"> {{ profile.public_repos }}</p>
     </div>
  </div>
  
  
    <ul v-if="repos" class="wrapper">
      <li v-for="repo in repos.slice(0, 4)" :key="repo.id">
        <a :href="repo.html_url" target="_blank">
           <div class="card">
            <div class="repos">
              {{repo.name}}  
            </div> 
            

          </div>
        </a>
      </li>
    </ul>

    <div v-if="loading">
      <img src="https://via.placeholder.com/50" alt="">
    </div>

   </div>
</template>

<script>
import api from '~/services/http'

export default {
  name: 'index',
  data() {
    return {
      user: '',
      profile: [],
      repos: [],
      loading: false
    }
  },
  methods: {
    async getUser() {
      this.loading = true
      await api.get(`users/${this.user}`).then(response => {
        this.profile = response.data

        this.getRepos()
      })
    },
    async getRepos() {
      await api.get(`users/${this.user}/repos`).then(response => {

        this.repos = response.data
        this.loading = false
      })
    }
  }
}
</script>


<style lang="css" scoped>


body, html {
  background-color: #eee;
}
*{
  overflow-x: hidden;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Raleway' , sans-serif;
}


.container {
  height: 100vh;
  width: 100vw;
  
}

.perfile h1{ 
  margin-top: 15px;
  font-weight: 500;
  letter-spacing: 5px;
  text-align: center;
  font-family: 'Vonique 43' , sans-serif;
  position: relative;
  
}

.perfile h1::before{
  position: absolute;
  content: '';
  bottom: 0;
  width: 360px;
  height: 2px;
  background-color: pink;
}
.input{
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 30px;
}

.input input{
  border: 0.2px solid rgba(0, 0, 0, 0.1);
  padding: 12px;
  margin-right: 12px;
  border: 2px solid rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  font-weight: bold;
}
.input button{
  padding: 12px;
  border-radius: 5px;
  border: 16px solid rgba(0, 0, 0, 1);
  border-style: none;
  transition: all 500ms;
  font-weight: bold;
}
.input button:hover{

  background-color: #CF9EFF;
}
.profile {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  
}

.profileimg{
  position: relative;
}
.profileimg p{
  position: absolute;
  bottom: 30px;
  left: 5px;
  width: 95%;
  background-color: #CF9EFF;
  text-align: center;
  font-weight: bold;
  font-family: 'Raleway' , sans-serif;
}
.profileavatar{
  display: flex;
  justify-content: center;
  align-items: center;
}

.profileavatar img{
  width: 25px;
  height: auto;
}
.profileavatar p{
  padding: 12px;
}
.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;

}
.wrapper li{
  list-style: none;
  text-decoration: none;
}
.wrapper a {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 25px;
  color: #CF9EFF;
  text-decoration: none;
}

.card {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #5A37CC;
  box-shadow: 0 0 10px rgba(0, 0, 0, .5);
  width: 250px;
  height: 100px;
  margin: 10px;

}

.profileimg img {
  width: 200px;
  height: auto;

  box-shadow: 0 0 5px rgba(0, 0, 0, .5);
  padding: 5px;
  background-color: #fff;
  margin-bottom: 25px;
}

</style>
