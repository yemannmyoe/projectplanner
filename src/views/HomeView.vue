<template>
  <div class="home" >
   <h1>Home</h1>
   <FilterNav @filterValue="current=$event" :current="current"></FilterNav>
   <div v-for="project in filterProjects" :key="project.id">
    <SingleProject :project="project" @delete="deleteProject" @complete="completeProject"></SingleProject>
  </div>
  </div>
  {{ current }}
</template>

<script>
// @ is an alias to /src

import SingleProject from '../components/SingleProject.vue';
import FilterNav from '../components/FilterNav.vue';

export default {
  name:'Home',
  components:{
    SingleProject,
    FilterNav
  },
  data(){
    return{
      projects:[],// Initial empty array for projects
      current:"all" //complete, ongoing
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
  
  computed:{
    filterProjects(){
      if(this.current==="complete"){
        return this.projects.filter((project)=>{
          return project.complete;
        })
      }
      if(this.current==="ongoing"){
        return this.projects.filter((project)=>{
          return !project.complete;
        })
      }
      return this.projects;
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
