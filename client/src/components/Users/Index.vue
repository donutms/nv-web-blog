<template>
    <div class="bg"><br>
    <div class="container blog-header"><br>
        <center><h2>ผู้ใช้งานในระบบ</h2></center>
        <h4>จำนวนผู้ใช้งาน {{users.length}}</h4>
        <p><button class="btn btn-outline-info" v-on:click="navigateTo('/user/create')">สร้างผู้ใช้งาน</button></p>
        <br>
        <div class="container" v-for="user in users" v-bind:key="user.id">
            <p>id: {{ user.id }}</p>
            <p>ชื่อ-นามสกุล : {{ user.name }} - {{ user.lastname }}</p>
            <p>email: {{ user.email }}</p>
            <p>password: {{ user.password }}</p>
                <button class="btn btn-outline-primary" v-on:click="navigateTo('/user/'+user.id)">ดูข้อมูล</button>
                <button class="btn btn-outline-warning" v-on:click="navigateTo('/user/edit/'+ user.id)">แก้ไขข้อมูล</button>
                <button  class="btn btn-outline-danger" v-on:click="deleteUser(user)">ลบข้อมูล</button>
            <hr>
        </div>
    </div>
    <br>
    </div>
</template>
<script>
    import UsersService from '@/services/UsersService'
    
    export default {
        data () {
            return {
                users: []
            }
        },
        async created () {
            this.users = (await UsersService.index()).data
        },
        methods: {
            navigateTo (route) {
                this.$router.push(route)
            },
            logout () {
                this.$store.dispatch('setToken', null)
                this.$store.dispatch('setUser', null)
                this.$router.push({
                    name: 'login'
                })
            },
            async deleteUser (user) {
                try {
                    await UsersService.delete(user)
                    this.refreshData()
                } catch (err) {
                    console.log(err)
                }
            },
                async refreshData() {
                this.users = (await UsersService.index()).data
            }
        }
    }
</script>
<style scoped>
.bg{
        background-color: rgb(230, 207, 178);
        width: 100%;
        height: 100%;
    }
.blog-header{
        background-color: rgb(245, 245, 245);
        
    }
</style>