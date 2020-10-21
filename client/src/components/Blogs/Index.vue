<template>
<div>
    <div class="component-wrapper">
 <div class="hero">
 
 <h1>Webblot from nodejs + vuejs Ebook</h1>
 <p>By Gooddev.ME</p>
<div class="clearfix"></div>
 </div>
 </div>


<div class="blog-header">
 <h2>ส่วนจดัการบลอ็ก</h2>
 <div>
 <form>
 <input type="text" v-model="search" placeholder="Search">
 </form>
</div>
 <div>
 <button v-on:click="navigateTo('/blog/create')">create
blog</button>
 <strong> จํานวน blog: </strong> {{blogs.length}}</div>
 <br>
 </div>
 <div v-if="blogs.length === 0" class="empty-blog">
 *** ไม่มีขอ้ มูล***
 </div>
 <div v-for="blog in blogs" v-bind:key="blog.id" class="blog-list">
 <!-- <p>id: {{ blog.id }}</p> -->
 <div class="blog-pic">
 <transition name="fade">
 <div class="thumbnail-pic" v-if="blog.thumbnail != 'null'">
 <img :src="BASE_URL+blog.thumbnail" alt="thumbnail">
 </div>
 </transition>
 </div>
 <h3>{{ blog.title }}</h3>
 <div v-html="blog.content.slice(0,200) + '...'"></div>
 <div class="blog-info">
 <p><strong>Category:</strong> {{ blog.category }}</p>
 <p><strong>Create:</strong> {{ blog.createdAt }}</p>
 <!-- <p>status: {{ blog.status }}</p> -->
 <p>
 <button v-on:click="navigateTo('/blog/'+ blog.id)">ดู blog</button>
 <button v-on:click="navigateTo('/blog/edit/'+ blog.id)">แกไ้ข blog</button>
 <button v-on:click="deleteBlog(blog)">ลบข้อมูล</button>
 </p>
 </div>
 <div class="clearfix"></div>
 </div>
 </div>
</template>
<script>
    import BlogsService from '@/services/BlogsService'
    export default {
        data () {
            return {
                search: '',
                blogs: [],
                BASE_URL: "http://localhost:8081/assets/uploads/",
                pictures: [],
            }
            
        },
        async created () {
            this.blogs = (await BlogsService.index()).data
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
            async deleteBlog (blog) {
                let result = confirm("Want to delete?")
                if (result) {
                    try {
                        await BlogsService.delete(blog)
                        this.refreshData()
                    } catch (err) {
                        console.log(err)
                    }
                }
            },
            async refreshData() {
                this.blogs = (await BlogsService.index()).data
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
 border:solid 1px #dfdfdf;
 margin-bottom: 10px;
 max-width: 900px;
 margin-left: auto;
 margin-right: auto;
 padding: 5px;
 box-shadow: 0 2px 4px 0 rgba(0,0,0,.1);
}
.blog-header {
 max-width: 900px;
 margin-left: auto;
 margin-right: auto;
}
.col1, .col2, .col3 {
 border:solid 1px red;
}
.categories li.clear a {
 background: tomato;
 color: white
}
.create-blog {
 margin-top: 10px;
}
@media (max-width: 768px) {
 .logo {
 width: 120px;
 }
}
.categories li a {
 padding: 5px 10px 5px 10px;
 background:paleturquoise; 
 color: black;
 text-decoration: none;
}
.blog-header {
 margin-top: 80px;
 max-width: 900px;
 margin-left: auto;
 margin-right: auto;
}
#blog-list-bottom{
 padding-top:4px;
}
.blog-load-finished{
 padding:4px;
 text-align: center;
 background: seagreen;
 color:white;
}
.categories {
 margin-top: 20px;
 padding: 0;
 list-style: none;
 float: left;
}
.categories li {
 float: left;
 padding: 2px;
}
.blog-list {
 border:solid 1px #dfdfdf;
 margin-bottom: 10px;
 max-width: 900px;
 margin-left: auto;
 margin-right: auto;
 padding: 5px;
 box-shadow: 0 2px 4px 0 rgba(0,0,0,.1);
}
</style>