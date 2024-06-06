<template>
  <div class="home" >
   <h1>Home</h1>
   <div v-for="project in projects" :key="project.id">
    <SingleProject :project="project" @delete="deleteProject" @complete="completeProject"></SingleProject>
  </div>
  </div>
</template>

<script>
// @ is an alias to /src

import SingleProject from '../components/SingleProject.vue';

export default {
  name:'Home',
  components:{
    SingleProject,
  },
  data(){
    return{
      projects:[] // Initial empty array for projects
    }
  },

  methods:{
    deleteProject(id){
      this.projects=this.projects.filter(project=>{
        return project.id!=id;
      })
    },
    completeProject(id){
      let findProject=this.projects.find(project=>{
        return project.id===id;
      });
      findProject.complete=!findProject.complete
    }
  },
  
 

  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response)=>{
      // console.log(response);
      return response.json()
    })
    .then((datas)=>{
      // console.log(datas);
      this.projects=datas
    })
    .catch(()=>{

    })
  }

}
</script>
