<template>
    <div class="container blog-wrapper" ><br>
        <h1>Edit User</h1>
        <form v-on:submit.prevent = "editUser">
            <p>ชื่อ: <input class="form-control" type="text" v-model="user.name"></p>
            <p>นามสกุล: <input class="form-control" type="text" v-model="user.lastname"></p>
            <p>email: <input class="form-control" type="text" v-model="user.email"></p>
            <p>รหัสผ่าน: <input class="form-control" type="text" v-model="user.password"></p>
            <p>
            <button class="btn btn-success" type="submit">บันทึก</button>
            <button class="btn btn-light" v-on:click="navigateTo('/users')">กลับ</button>
            </p>
        </form>
        <hr>
        <div class="container blog-wrapper" ><br>
            <p>ชื่อ: {{ user.name }}</p>
            <p>นามสกุล: {{ user.lastname }}</p>
            <p>email: {{ user.email }}</p>
            <p>รหัสผ่าน: {{ user.password }}</p>
            <p></p>
        </div>
    </div>
</template>
<script>
import UsersService from '@/services/UsersService'

export default {
    data () {
    return {
        user: {
            name: '',
            lastname: '',
            email: '',
            password: '',
            status: 'active'
        }
    }
    },
    methods: {
        async editUser () {
            try {
                await UsersService.put(this.user)
                this.$router.push({
                    name: 'users'
                })
            } catch (err) {
                console.log(err)
            }
        }
    },
    async created () {
        try {
            let userId = this.$route.params.userId
            this.user = (await UsersService.show(userId)).data
        } catch (error) {
            console.log (error)
        }
    }
}
</script>
<style scoped>
</style>