<template>
    <div class="container blog-wrapper">
        <center><h1>เเสดงสินค้า</h1></center>
        id : <p class="form-control"> {{ product.id }}</p>
        ชื่อ : <p class="form-control"> {{ product.name }}</p>
        รสชาติ : <p class="form-control"> {{ product.taste }}</p>
        ราคา : <p class="form-control"> {{ product.price }}</p>
        สถานะ :<p class="form-control"> {{ product.status }}</p>
        <p>
        <button class="btn btn-success" v-on:click="navigateTo('/products')">กลับ </button>
        </p>
    </div>
</template>
<script>
    import ProductService from '@/services/ProductService'
    export default {
        data () {
            return {
                product: null
            }
        },
        async created () {
            try {
                let productId = this.$route.params.productId
                this.product = (await ProductService.show(productId)).data
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