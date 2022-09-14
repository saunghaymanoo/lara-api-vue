<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light container">
  <router-link class="navbar-brand" to="/">Navbar</router-link>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav ms-auto">
      <li class="nav-item active">
        <router-link class="nav-link" to="/">Home</router-link>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="/about">About</a>
      </li>
      <template v-if="auth===null">
      <li class="nav-item">
        <a class="nav-link" href="/login">Login</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="/register">Register</a>
      </li>
      </template>
       <template v-else>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Product
              </a>
              <ul class="dropdown-menu">
                <li><router-link class="dropdown-item" to="/product/create">Create Product</router-link></li>
                <li><router-link class="dropdown-item" to="/product">Product List</router-link></li>
              </ul>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                {{ auth.name }}
              </a>
              <ul class="dropdown-menu">
                <li><router-link class="dropdown-item" to="/dashboard">Dashboard</router-link></li>
                <li><hr class="dropdown-divider"></li>
                <li><a class="dropdown-item" href="#" @click="logout()">Logout</a></li>
              </ul>
            </li>
          </template>
    </ul>
  </div>
</nav>
</template>

<script>
import { mapGetters, mapState } from 'vuex'
    export default {
        computed: {
           ...mapState([
            "auth","token"
           ]),
           ...mapGetters([
            "getUrl"
           ])
        },
        methods: {
            logout() {
                  let headers = new Headers();
                  headers.append('Authorization','Bearer '+this.token);
                  fetch(this.getUrl('/logout'),{
                    method : 'POST',
                    headers
                  }).then(res => res.json())
                  .then(json => {
                    if(json.success == true){
                      localStorage.removeItem('auth');
                      localStorage.removeItem('token')
                      this.$store.dispatch('logout');
                      this.$router.push('/login');
                    }
                  })
                  .finally(_=>this.$router.push('/login'));      
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>