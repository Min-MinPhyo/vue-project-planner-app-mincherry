<template>
<div class="project" :class="{complete:project.complete}">

    <div class="flexing">
        <div>
            <h3 @click="showDetail =!showDetail">{{project.title}}</h3>
      
        </div>

        <div>
            <span class="material-icons" @click="deleteTask">
                delete
            </span>

            <router-link :to="{name:'EditProject',params:{id:project.id}}">


 <span class="material-icons">
                edit
            </span>


            </router-link>
             <span class="material-icons" @click="updateCompleteTask">
                done
            </span>

        </div>
    </div>

    <p v-if="showDetail">{{project.detail}}</p>


        


</div>

</template>

<script>
export default {
    props:['project'],
    data(){
        return {
            showDetail:false,
            api:'http://localhost:3000/projects/'
        }
        
        },
        methods:{
            deleteTask(){
              const deleteAPI=this.api+this.project.id;
            //   console.log(deleteAPI);
            //   return
              fetch(deleteAPI,{method:"DELETE"})
              .then(()=>{
                this.$emit('delete',this.project.id)

              })
              .catch((error)=>{
                console.log(error.message)
              })
            },
            updateCompleteTask(){
                const updateCompleteAPI=this.api+this.project.id;
                fetch(updateCompleteAPI,{
                    method:"PATCH",
                    headers:{
                       "Content-Type":"application/json"
                    },
                    body:JSON.stringify({
                          complete:!this.project.complete
                    })
                })
                .then(()=>{
                    this.$emit("complete",this.project.id);
                })
                .catch((error)=>{
                    console.log(error.message());
                })
            }
        }

}
</script>

<style scoped>
.project{
    background:#7777;
    padding:20px;
    margin:10px;
    border-radius:10px;
    border-left: 7px solid crimson;
    
    
}
h3{
    color:indigo;
    cursor: pointer;
}
.flexing{
    display:flex;
    justify-content:space-between;
    align-items:center
}
span{
    margin-left:10px;
     cursor: pointer;

}
span:hover{
    color:green;
    transform: rotate(20deg);

}
.complete{
    border-left-color: green;
}

</style>