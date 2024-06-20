<template>
<h3>Home</h3>
<Filter @filterValue="current=$event" :current="current"></Filter>
<div v-for="project in filterProjects" :key="project.id">

<SinglePage :project="project" @delete="deleteProject" @complete="completeProject"></SinglePage>
</div>
{{ current }}
</template>

<script>

import Filter from '../components/Filter'
import SinglePage from '../components/SinglePage'
export default {
  name: 'HomeView',
  data(){
    return{
      projects:[],
      current:'all'//all complete ongoing

    }
  },
  computed:{
    filterProjects(){
      if(this.current==='complete'){
        return this.projects.filter((project)=>project.complete)
        
      }
      if(this.current==='ongoing'){
        return this.projects.filter((project)=>!project.complete)
      }

      return this.projects

    }


  },
  methods:{

    deleteProject(id){
    
      this.projects=this.projects.filter((project)=>{
        return project.id !=id
      })
;    },
completeProject(id){
   let findProject=this.projects.find((project)=>{
    return project.id===id
   });
   findProject.complete=!findProject.complete

}
  },

  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response)=>{
      return response.json();
    })
    .then((datas)=>{
      this.projects=datas;

    })
    .catch((error)=>{
      console.log(error.message());
    })

  },
  components: {
    Filter,
    SinglePage,
  
  }
}
</script>
