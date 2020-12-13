<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @complete="toggleProjectState"
                       @delete="handleDelete"/>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import SingleProject from "../components/SingleProject";

export default {
  name: 'Home',
  components: {
    SingleProject,
  },
  data(){
    return{
      projects: []
    }
  },
  methods: {
    handleDelete(id){
      this.projects = this.projects.filter(project => project.id !== id);
    },
    toggleProjectState(id){
      const updatedProject = this.projects.find(project => project.id === id);
      updatedProject.complete = !updatedProject.complete

    }
  },
  mounted() {
    axios.get('http://localhost:3000/projects').then(res => {

      const resData = res.data;
      this.projects = resData;

    }).catch(err => {
      console.log(err);
    });
  },

}
</script>
