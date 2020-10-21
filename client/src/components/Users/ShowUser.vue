<template>
    <div class="container blog-wrapper" >
        <h1>เเสดงผู้ใช้งาน</h1>
        id: <p class="form-control" > {{ user.id }}</p>
        ชื่อ-นามสกุล :<p class="form-control"> {{ user.name }} - {{ user.lastname }}</p>
        Email :<p class="form-control"> {{ user.email }}</p>
        รหัสผ่าน :<p class="form-control"> {{ user.password }}</p>
        <p>
        <button class="btn btn-success" v-on:click="navigateTo('/users')">กลับ </button>
        </p>
    </div>
</template>
<script>
import UsersService from '@/services/UsersService'

export default {
    data () {
        return {
            user: null
        }
    },
    async created () {
        try {
            let userId = this.$route.params.userId
            this.user = (await UsersService.show(userId)).data
        } catch (error) {
            console.log (error)
        }
    },
     methods : {
            navigateTo (route) {
                this.$router.push(route)
            },
        }
}
</script>
<style scoped>
</style>