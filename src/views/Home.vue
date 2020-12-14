<template>
  <div class="home">
    <FilterNav @filterChange="changeFilter" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @complete="toggleProjectState"
                       @delete="handleDelete"/>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import SingleProject from "../components/SingleProject";
  import FilterNav from "../components/FilterNav";

export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  data(){
    return{
      projects: [],
      current: 'all'
    }
  },
  methods: {
    handleDelete(id){
      this.projects = this.projects.filter(project => project.id !== id);
    },
    toggleProjectState(id){
      const updatedProject = this.projects.find(project => project.id === id);
      updatedProject.complete = !updatedProject.complete

    },
    changeFilter(text){
      this.current = text;
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
  computed: {
    filteredProjects(){
      return this.projects.filter(project => {
        if(this.current === 'all' ){
          return true;
        }
        if(this.current === 'completed'){
          if(project.complete){
            return true;
          }else{
            return false;
          }
        }
        if(this.current === 'ongoing'){
          if(project.complete){
            return false;
          }else{
            return true;
          }
        }
      })
    }
  }

}
</script>
