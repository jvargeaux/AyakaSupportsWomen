<template>
  <div class="nav">
    <div class="bgOverlay"></div>
    <div class="bg"></div>
    <div class="accountStatus" v-if="user">
      <h6>Welcome, {{ user.name }}</h6>
      <button v-on:click="logout">Logout</button>
    </div>
    <div class="accountStatus" v-else>
      <h6><a class="textLink" href=".#/login">Login</a> or <a class="textLink" href=".#/register">Create An Account</a></h6>
    </div>
    <a href="./"><h1>Cafe Tour Tokyo</h1></a>
    <nav>
      <ul>
        <li><router-link to="/">Home</router-link></li>
        <li><router-link to="/about">About</router-link></li>
        <li><router-link to="/list">Cafe List</router-link></li>
        <!-- <li><router-link to="/map">Map</router-link></li> -->
        <li v-if="user && user.email === 'fakeAdmin@gmail.com'"><router-link to="/api">API</router-link></li>
        <li v-if="user"><router-link to="/profile">Profile</router-link></li>
        <!-- <li><router-link to="/login">Login</router-link></li> -->
      </ul>
    </nav>
  </div>
</template>

<script>
  const DASHBOARD_URL = window.location.hostname === 'localhost' ?
    'http://' + window.location.hostname + ':5000/dashboard' :
    'https://' + window.location.host + '/dashboard';
  
  export default {
    name: 'Navigation',
    props: {

    },
    data() {
      return {
        user: null
      }
    },
    methods: {
      checkLoginStatus: function() {
        fetch(DASHBOARD_URL, {
          method: "GET",
          headers: {
            'Content-Type': 'application/json',
            'Authorization': document.cookie
          }
        })
         .then(res => res.json())
         .then(data => {
           console.log(data);
           this.user = data;
         })
         .catch(err => console.log(err));
      },
      logout: function() {
        document.cookie = null;
        location.reload();
      }
    },
    mounted() {
      this.checkLoginStatus();
    }
  }
</script>

<style scoped>

  .nav {
    width: 100vw;
  }
  .bg {
    position: absolute;
    width: 100vw;
    height: 40vh;
    background-image: url("../assets/cafe1.jpg");
    background-position: 50% 50%;
    background-repeat: no-repeat;
    background-size: cover;
    z-index: -99;
  }
  .bgOverlay {
    position: absolute;
    width: 100vw;
    height: 40vh;
    /* background-color: rgba(12,121,61,.6); */
    background: linear-gradient(0deg, rgba(8,154,74,.4), rgba(22,83,49,.6));
    z-index: -80;
  }

  .accountStatus {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    right: 5vw;
    top: 1vh;
  }

  .textLink {
    color: rgba(179,249,236,.75);
  }
  .textLink:hover {
    color: rgba(255,255,255,1);
  }

  h6 {
    text-align: center;
    margin-right: 2vw;
    font-size: 1.15rem;
    font-weight: 500;
    color: rgba(255,255,255,.75);
    text-shadow: 0px 0px 15px rgba(0,0,0,1);
    cursor: default;
  }
  button {
    outline: none;
    border: none;
    height: 2.5rem;
    width: 7rem;
    box-sizing: border-box;
    border: 1px solid #9f9f9f;
    border-radius: 6px;
    font-size: 0.9rem;
    color: #b8b8b8;
    border: 1px solid #9f9f9f;
    background-color: rgba(0,0,0,.25);
    transition: color 0.3s, background 0.3s, border 0.3s;
  }
  button:hover {
    cursor: pointer;
    color: #e8e8e8;
    border: 1px solid #e8e8e8;
    background-color: rgba(0,0,0,.5);
  }

  h1 {
    font-family: 'Julius Sans One', sans-serif;
    font-size: 52px;
    color: #fff;
    text-shadow: 0px 0px 20px rgba(0,0,0,.5);
    padding-top: 9vh;
    text-align: center;
    margin-bottom: 6vh;
  }

  a {
    color: inherit;
  }

  nav {
    padding-bottom: 75px;
  }

  nav ul {
    display: flex;
    justify-content: center;
  }

  nav ul li {
    list-style-type: none;
  }

  nav ul li a {
    color: #fff;
    font-size: 18px;
    font-weight: 500;
    padding: 12px 36px;
    background-color: rgba(46,46,46,.5);
  }

  nav ul li a:hover {
    color: #131313;
    background-color: #f9f9f9;
    cursor: pointer;
  }


</style>
