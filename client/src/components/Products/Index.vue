<template >
<div class="bg"><br>
<div class="container blog-header">
<div><br>
 <center><h2>สินค้า</h2></center>
 <div>
 <form>
 <input class="form-control mr-sm-2" type="text" v-model="search" placeholder="Search">
 </form>
</div>
 <div><br>
 <button  class="btn btn-outline-info" v-on:click="navigateTo('/product/create')">สร้างสินค้า</button>
 <strong> จํานวนสินค้า: </strong> {{products.length}}</div>
 <br>
 </div>
<div>
 <div v-if="products.length === 0" class="empty-blog">
 *** ไม่มีข้อมูลในระบบ*** 
 </div><br>
</div>

 <div v-for="product in products" v-bind:key="product.id" class="blog-list">
 <!-- <p>id: {{ blog.id }}</p> -->
 <div class="blog-pic">
 <transition name="fade">
 <div class="thumbnail-pic" v-if="product.thumbnail != 'null'">
 <img :src="BASE_URL+product.thumbnail" alt="thumbnail">
 </div>
 </transition>
 </div>

 <h3>{{ product.name }}</h3>
 <p><strong>รสชาติ:</strong> {{ product.taste }}</p>
 <p><strong>สถานะ:</strong> {{ product.status }}</p>
 <p><strong>วันเวลาที่นำเข้าสินค้า:</strong> {{ product.createdAt }}</p>
 <!-- <p>status: {{ blog.status }}</p> -->
 <p>
 <button  class="btn btn-outline-primary" v-on:click="navigateTo('/product/'+ product.id)">ดูข้อมูลสินค้า</button>
 <button class="btn btn-outline-warning"  v-on:click="navigateTo('/product/edit/'+ product.id)">เเก้ไขสินค้า</button>
 <button class="btn btn-outline-danger" v-on:click="deleteBlog(product)">ลบข้อมูล</button>
 </p>
 </div>

 <div class="clearfix"></div>
 
 </div>
 <br><br><br>
</div>

</template>
<script>
    import ProductService from '@/services/ProductService'
    import _ from 'lodash'
    export default {
        data () {
            return {
                search: '',
                products: [],
                BASE_URL: "http://localhost:8081/assets/uploads/",
                pictures: [],
            }
            
        },
        watch: {
                search: _.debounce(async function (value) {
                const route = {
                    name: 'products'
                }
                if(this.search !== '') {
                    route.query = {
                        search: this.search
                    }
                }

                console.log('search: ' + this.search)
                this.$router.push(route)
                }, 700),

                '$route.query.search': {
                    immediate: true,
                    async handler (value) {
                        this.products = (await ProductService.index(value)).data
                    }
                }
            },
        async created () {
            this.products = (await ProductService.index()).data
        },
        methods: {
            logout () {
                this.$store.dispatch('setToken', null)
                this.$store.dispatch('setBlog', null)
                this.$router.push({
                    name: 'login'
                })
            },
            navigateTo (route) {
                this.$router.push(route)
            },
            async deleteBlog (product) {
                let result = confirm("Want to delete?")
                if (result) {
                    try {
                        await ProductService.delete(product)
                        this.refreshData()
                    } catch (err) {
                        console.log(err)
                    }
                }
            },
            async refreshData() {
                this.products = (await ProductService.index()).data
            }
        }
    }
</script>
<style scoped>
.empty-blog {
 width: 100%;
 text-align: center;
 padding:10px;
 background:darksalmon;
 color:white;
}
/* thumbnail */
.thumbnail-pic img{
 width: 200px;
 padding: 5px 10px 0px 0px;
}
.blog-info {
 float: left;
}
.blog-pic {
 float: left;
}
.clearfix {
 clear: both;
}
.blog-list {
 background-color: rgb(245, 245, 245);
 border:solid 1px #c9c5c5;
 margin-bottom: 10px;
 max-width: 900px;
 margin-left: auto;
 margin-right: auto;
 padding: 5px;
 box-shadow: 0 2px 4px 0 rgba(0,0,0,.1);
}

.bg{
        background-color: rgb(230, 207, 178);
        width: 100%;
        height: 100%;
    }
.blog-header{
        background-color: rgb(245, 245, 245);
        
    }
</style>