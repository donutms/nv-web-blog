<template>
<div class="bg"><br>
<div class="container blog-header" style="width: 30%">
        <center><h1>User Login</h1></center>
        <form v-on:submit.prevent="onLogin">
            <p>Username: <input class="form-control" type="text" v-model="email" /></p>
            <p>Password: <input  class="form-control" type="password" v-model="password" /></p>
            <p><center><button class="btn btn-success" type="submit">Login</button></center></p>
            <div class="error" v-if="error">{{error}}</div>
        </form>
    </div>
 </div>
</template>
<script>
import AuthenService from '@/services/AuthenService'

export default {
    data () {
        return {
            email: '',
            password: '',
            error: null
        }
    },
    methods: {
        async onLogin () {
            try {
                const response = await AuthenService.login({
                    email: this.email,
                    password: this.password
                })

                this.$store.dispatch('setToken', response.data.token)
                this.$store.dispatch('setUser', response.data.user)
                this.$router.push({
                    name: 'users'
                })

                console.log(response)
            } catch (error) {
                console.log(error)
                this.error = error.response.data.error
                this.email = ''
                this.password = ''
            }
        }
    }
}
</script>
<style scoped>
    .error {
        color:red;
        
    }
</style>