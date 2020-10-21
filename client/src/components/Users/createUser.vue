<template>
    <div class="container blog-wrapper">
        <h1>สร้างผู้ใช้งาน</h1>
        <form v-on:submit.prevent = "createproduct">
      <p>
        <label class="control-label">ชื่อ: </label>
        <input type="text" v-model="user.name" class="form-control">        
      </p>
      <p>
        <label class="control-label">นามสกุล :</label>
        <input type="text" v-model="user.lastname" class="form-control">
      </p>
      <p>
        <label class="control-label">Email :</label>
        <input type="text" v-model="user.email" class="form-control">
      </p>
      <p>
        <label class="control-label">รหัสผ่าน :</label>
        <input type="text" v-model="user.password" class="form-control">
      </p>
      <p>
        <button class="btn btn-success" type="submit">สร้างผู้ใช้งาน</button>
        <button class="btn btn-default" type="button" v-on:click="navigateTo('/users')">กลับ</button>
      </p> 
        </form>
        <hr>
        <div>
            <p>ชื่อ: {{ user.name }}</p>
            <p>นามสกุล: {{ user.lastname }}</p>
            <p>email: {{ user.email }}</p>
            <p>รหัสผ่าน: {{ user.password }}</p>
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
        async createproduct () {
            try {
                await UsersService.post(this.user)
                this.$router.push({
                    name: 'users'
                })
            } catch (err) {
                console.log(err)
            }
        },
        navigateTo (route) {
             this.$router.push(route)
        },
    }
}
</script>
<style scoped>
</style>