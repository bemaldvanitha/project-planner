<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input type="text" required v-model="title"/>

    <label>Detail:</label>
    <textarea required v-model="details"></textarea>

    <button>Add Project</button>
  </form>
</template>

<script>
  import axios from "axios";
  export default {
    name: "AddProject",
    data(){
      return{
        title: '',
        details: '',
        url: 'http://localhost:3000/projects',
      }
    },
    methods: {
      handleSubmit(){

        axios.post(this.url,{

          title: this.title,
          details: this.details,
          complete: false

        }).then(res => {

          this.$router.push('/')

        }).catch(err => {
          console.log(err);
        });

      }
    }
}
</script>

<style scoped>
  form{
    background: white;
    padding: 20px;
    border-radius: 10px;
  }
  label{
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0;
  }
  input{
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
  }
  textarea{
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
  }
  form button{
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
</style>