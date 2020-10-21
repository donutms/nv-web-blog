<template>
    <div >
        <div class="nv-navbar ">
        <div class="container">
            <ul class="nav "><br>
                <img src="../assets/Logo2.png" style="
                padding: 0px 0;
                margin-top: -15px;
                width: 50px;
                height: 50px;"    >
                <li><router-link :to="{name: 'products'}" >Product</router-link></li>
                <li><router-link :to="{name: 'users'}" >Users</router-link></li>
            </ul>
            <ul class="nav" style="float : right">
            <li><a v-on:click.prevent="logout" href="#">Logout</a></li>
            </ul>
            <div class="clearfix"></div>
        </div>
        </div>
    </div>

</template>
<script>
export default {
    methods: {
        logout () {
            this.$store.dispatch('setToken', null)
            this.$store.dispatch('setUser', null)
            this.$router.push({
                name: 'login'
            })
        }
    }
}
</script>
<style scoped>
    .logo {
        padding: 0;
        width: 60px;
        float:left;
    }
    .nv-navbar {
        background-color:rgb(117, 63, 63);
        width: 100%;
        height: 60px;
        padding:10px 0px 10px 0px;
    }
    .nv-navbar .nav {
        list-style: none;
        margin:0;
        padding:8px;
        float:left;
    }
    .nv-navbar .navv {
        list-style: none;
        margin:0;
        padding:0;
        float:right;
    }
    .nv-navbar .nav li {
        float:right;
    }
    .nv-navbar .nav li a {
        padding: 10px;
        text-decoration: none;
        border-radius: 5px;
        color:white;
        font-weight: bold;
    }
    .nv-navbar .nav li a:hover {
        padding: 10px;
        text-decoration: none;
        color:rgb(0, 0, 0);
    }
    .nv-navbar .nav li a.router-link-active {
        border-radius: 5px;
        background-color:rgb(252, 238, 238);
        color:rgb(0, 0, 0);
    }
    .clearfix {
        clear: left;
        
    }
.navbar-brand > img {

  padding: 0px 0;
  margin-top: -20px;
  width: 50px;
  height: 50px;
}
.navbar-inverse {
  background-color: rgb(11, 138, 212);
}
.navbar-inverse .navbar-nav > li > a {
  color: #dbdbf6;
}
</style>