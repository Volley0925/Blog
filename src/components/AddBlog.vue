<template>
  <div id="add-blog">
    <h2>添加博客</h2>
    <form v-if="!submmited">
      <label>博客标题</label>
      <!-- v-model对title进行绑定 实时监听-->
      <input type="text" v-model="blog.title" required/>

      <label>博客内容</label>
      <!--textarea多行文本-->
      <textarea v-model="blog.content"></textarea>

      <div id="checkboxes">
        <label>Vue.js</label>
        <!--通过v-model双向绑定 复选框选择的内容会存入到data中的categories数组中-->
        <input type="checkbox" value="Vue.js" v-model="blog.categories">
        <label>Node.js</label>
        <input type="checkbox" value="Node.js" v-model="blog.categories">
        <label>React.js</label>
        <input type="checkbox" value="React.js" v-model="blog.categories">
        <label>Angular4</label>
        <input type="checkbox" value="Angular4" v-model="blog.categories">
      </div>
        <label>作者</label>
        <select v-model="blog.author">
          <option v-for="author in authors">{{author}}</option>
        </select>
        <button v-on:click.prevent="post" >添加博客</button>

    </form>
    <div v-if="submmited">
      <h3>你的博客发送成功！</h3>
    </div>
    <hr>
    <div id="preview"> 
      <h3>博客总览</h3>
      <p>博客标题：{{blog.title}}</p>
      <p>博客内容：</p>
      <p>{{blog.content}}</p>
      <p>博客分类：</p>
      <ul>
        <li v-for="category in blog.categories">{{category}}</li>
      </ul>
      <p>作者：</p>
      <p>{{blog.author}}</p>
    </div>
   
  </div>
</template>

<script>
export default {
  //https://jsonplaceholder.typicode.com/
  //https://jsonplaceholder.typicode.com/posts
  name: 'add-blog',
  data () {
    return {
      blog:{
        title:"",
        content:"",
        categories:[],
        author:""
      },
      authors:["Hemiah","Henry","Bucky"],
      submmited:false
      
    }
  },
  methods:{
    post:function(){
      this.$http.post("https://vuedemo-38f20.firebaseio.com/posts.json",this.blog)
      .then(function(data){
        console.log(data);
        this.submmited=true;
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #add-blog *{
    box-sizing: border-box;
  }

  #add-blog{
    margin: 20px auto;
    max-width: 600px;
    padding: 20px;
  }

  lable{
    display: block;
    margin: 20px 0 10px
  }

  input[type="text"],textarea,select{
    display: block;
    width: 100%;
    padding: 8px;
  }

  textarea{
    height: 200px;
  }

  #checkboxes label{
    display: inline-block;
    margin-right:10px; 
  }

  button{
    display: block;
    margin:20px 0;
    background: crimson;
    color: #fff;
    border: 0;
    padding: 14px;
    border-radius: 4px;
    font-size: 18px;
    cursor: pointer;
  }

  #preview{
    padding: 10px 20px;
    border: 1px dotted #ccc;
    margin: 30px 0;
  }

h3{
  margin-top: 10px;
}
</style>
