<template>
  <div v-theme:column="'narrow'" id="show-blogs">
      <h1 >博客总览</h1>
      <input type="text" v-model="search" placeholder="搜索">
      <div  class="single-blog" v-for="blog in filteredBlogs">
      	<router-link v-bind:to="'/blog/'+blog.id">
          <h2 v-rainbow>
          {{blog.title | to-uppercase}}
        </h2>
        </router-link>
      	<article>
      		{{blog.body | snippet}}
      	</article>
      </div>
  </div>
</template>

<script>
export default {
  name: 'show-blogs',
  data(){
  	return {
  		blogs:[],
      search:""
  	}
  },
  created(){
  	this.$http.get("./../static/posts.json").then(function(data){
  			// console.log(data);
  			this.blogs=data.body.slice(0,10);
  			// console.log(this.blogs)
  	})
  },
  computed:{
    filteredBlogs:function(){
      return this.blogs.filter((blog)=>{
        return blog.title.match(this.search)
      })
    }
  },
  filters:{
    "to-uppercase":function(value){
      return value.toUpperCase();//局部过滤器
    }
  },
  directives:{
    'rainbow':{
      bind:function(el,binding,vnode){
      el.style.color="#"+Math.random().toString(16).slice(2,8);
      }
    }
  }
}
</script>

<style scope>
 #show-blogs{
 	max-width:800px;
 	margin: 0 auto;
 }
 .single-blog{
 	padding: 20px;
 	margin: 20px 0;
 	box-sizing: border-box;
 	background: #eee;

 }
 #show-blogs a{
  color: #444;
  text-decoration: none;
 }
 input[type="text"]{
  padding: 8px;
  width: 20%;
 }
</style>
