<template>
   <h2>Edit Project</h2>
  <form  @submit.prevent="addProject">


        <label >Project Title</label>
        <input type="text" v-model="title">

        <label>Project Details</label>
        <input type="text" v-model="detail">
        <button @click="updateProject">Update Project</button>
  </form>
</template>

<script>
export default {
  name:'EditProject',
  props:["id"],
  data(){
    return {
      title:'',
      detail:''

    }
    
    
    },
    methods:{
     updateProject(){
       fetch('http://localhost:3000/projects/'+this.id,{
        method:"PATCH",
        headers:{
          "Content-Type":"application/json"
        },
        body:JSON.stringify({
          title: this.title,
          detail: this.detail
        })
       })
       .then((res)=>{
        this.$router.push({name:'Home'})
       })
       .catch((err)=>{
        console.log(err.message)
       })
     }
    },
    mounted(){

      fetch('http://localhost:3000/projects/'+this.id)
      .then((response)=>{
        return response.json()
      })
      .then((data)=>{
        this.title=data.title,
        this.detail=data.detail
      })
      .catch((error)=>{
        console.log(error.message())
      })

    }

}
</script>

<style>

</style>