<template>
  
  <form @submit.prevent="addProject">
    <h1>Edit Project</h1>
    <label>Project Title</label>
    <input type="text" v-model="title">
    <label>Project Detail</label>
    <input type="text" v-model="detail">
    <button @click="updateProject">Update Project</button>
 </form>
</template>

<script>
import HomeView from './HomeView.vue';

export default {
    props:["id"],

    data(){
        return{
            title:"",
            detail:""
        }
    },
    mounted(){
        fetch('http://localhost:3000/projects/'+this.id)
        .then((res)=>{
            return res.json()
            
            // console.log(res);
        })
        .then((datas)=>{
            // console.log(datas);
            this.title=datas.title;
            this.detail=datas.detail;
        })
        .catch((err)=>{
            console.log(err);
        })
    },
    methods:{
        updateProject(){
           fetch('http://localhost:3000/projects/'+this.id,{
            method: "PATCH",
            headers:{
                "Content-Type":"application/json"
            },
            body:JSON.stringify(
                {
                    title:this.title,
                    detail:this.detail
                }
            )
         } )
         .then((res)=>{
            this.$router.push({name:"home"})
         })
         .catch((err)=>{
            console.log(err);
         })
        }
    }
}
</script>

<style>

</style>