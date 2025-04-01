<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  <div id="demo" @click="changemsg()">click</div>
  </div>
</template>

<script>
export default {

 data(){
  return {
    msg:"hello siyamak"
  }
 },
 methods:{
  changemsg(){
    this.msg="hello world"
  }
 }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
