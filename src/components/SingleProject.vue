<template>
  <div class="project" :class="{complete: project.complete}">

    <div class="actions">
      <h3 @click="toggleDetailVisibility">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="'/projects/' + project.id">
          <span class="material-icons">create</span>
        </router-link>
        <span @click="deleteProject" class="material-icons">delete</span>
        <span @click="toggleProjectState" class="material-icons tick">done_outline</span>
      </div>
    </div>

    <div class="details" v-if="showDetail">
      <p>{{ project.details }}</p>
    </div>

  </div>
</template>

<script>
  import axios from "axios";
export default {
  name: "SingleProject",
  props: {
    project: {
      id: Number,
      title: String,
      details: String,
      complete: Boolean
    }
  },
  data(){
    return{
      showDetail: false,
      uri: 'http://localhost:3000/projects/' + this.project.id,
    }
  },
  methods: {
    toggleDetailVisibility(){
      this.showDetail = !this.showDetail
    },
    deleteProject(){
      axios.delete(this.uri).then(() => {

        this.$emit('delete',this.project.id);

      }).catch(err => {
        console.log(err);
      });
    },
    toggleProjectState(){
      axios.patch(this.uri,{
        complete: !this.project.complete
      }).then(res => {

        this.$emit('complete',this.project.id);

      }).catch(err => {
        console.log(err);
      })
    },
  }
}
</script>

<style scoped>
  .project{
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 4px solid #e90074;
  }
  h3{
    cursor: pointer;
  }
  .actions{
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .material-icons{
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
  }
  .material-icons:hover{
    color: #777;
  }
  .project.complete{
    border-left: 4px solid #00ce89;
  }
  .project.complete .tick{
    color: #00ce89;
  }

</style>