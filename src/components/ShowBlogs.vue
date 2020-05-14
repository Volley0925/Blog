<template>
  <div id="show-blogs" v-theme:column="'narrow'">
    <h1>博客总览</h1>
    <input type="text" v-model="search" placeholder="搜索">
    <div v-for="blog in filteredBlogs" class="single-blog">
    	<!-- | to-uppercase将标题变成大写 （过滤器）-->
    	<router-link v-bind:to="'/blog/'+blog.id"><h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
    	</router-link>
    	<article>
    		<!--| snippet省略展示的内容-->
    		{{blog.content | snippet}}
    	</article>
    </div>
  </div>
</template>

<script>


export default {
  name: 'show-blogs',
  data(){
  	return{
  		blogs:[],
  		search:""

  	}
  },
  created(){
  	this.$http.get('https://vuedemo-38f20.firebaseio.com/posts')
  	.then(function(data){
  		console.log(data.json());
      return data.json()
  		// this.blogs=data.body.slice(0,10);

  	})
    .then(function(data){
      var blogsArray=[];
      //建一个数组 然后遍历 data里面有多少个key就遍历多少
      for(let key in data){
        console.log(key);
        //输入的是唯一标识的那串id
        console.log(data[key]);        
        //data[key]那个每一个标识对应的内容
        data[key].id=key;
        //在内容里多了一个id属性 将key这个唯一标识符传给id
        blogsArray.push(data[key]);
        //将data[key]里的内容push到数组里面
        console.log(blogsArray);
        this.blogs=blogsArray;
        console.log(this.blogs)

      }
    })
  },
  computed:{
  	filteredBlogs:function(){
  		return this.blogs.filter((blog)=>{
  			return blog.title.match(this.search)
  		})

  	}
  },
  //定义多个过滤器
  filters:{
  	// "to-uppercase":function(value){
  	// 	return value.toUpperCsae();
  	// }
  	// toUpperCsae(value){
  	// 	return value.toUpperCsae();
  	// }
  },
  //定义多个钩子函数（局部）
  directives:{
  	'rainbow':{
  		bind(el,binding,vnode){
  			el.style.color="#" + Math.random().toString(16).slice(2,8);
  		}
  	}
  }
}
</script>

<style>
	#show-blogs{
		max-width: 800px;
		margin:0 auto;
	}
	.single-blog{
		padding: 20px;
		margin: 20px 0;
		box-sizing: border-box;
		background: #eee;
		border:1px dotted #aaa;
	}
	#show-blogs a{
		color: #444;
		text-decoration: none;
	}
	input[type="text"]{
		padding: 8px;
		width: 100%;
		box-sizing: border-box;

	}
  
</style>
