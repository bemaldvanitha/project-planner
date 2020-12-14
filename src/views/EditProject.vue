<template>

  <form @submit.prevent="editProject">
    <label>Title:</label>
    <input type="text" required v-model="title"/>

    <label>Detail:</label>
    <textarea required v-model="details"></textarea>

    <button>Edit Project</button>
  </form>

</template>

<script>
  import axios from "axios";

  export default {
    name: "EditProject",
    data(){
      return{
        title: '',
        details: '',
        id: '',
        url: 'http://localhost:3000/projects',
      }
    },
    mounted() {
      this.id = this.$route.params.id;

      axios.get(this.url + '/' + this.id).then(res => {
        const resData = res.data;

        this.title = resData.title;
        this.details = resData.details;
      });
    },
    methods: {
      editProject(){
        axios.patch(this.url + '/' + this.id,{
          title: this.title,
          details: this.details,
        }).then(res => {

          this.$router.back();

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