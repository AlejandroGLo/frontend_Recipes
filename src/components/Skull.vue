<template>
    <div>
        <img class="center" src="../assets/skull.jpeg" alt="skull"></img>
        <h1>Skull</h1>
        
        <div class="btn">
        <button v-on:click=gotohome() type="button" class="btn btn-secondary" >
            go back
        </button>
        
        </div>
        <p>{{ msg }}</p>
    </div>
</template>
 
<script>
import axios from 'axios'
export default
{
    name: 'Skull',
    data()
    {
        return {
            msg: 'Hi! This is the skull component 💀'
        }
    },
    methods:
    {
        getSkull()
        {
            axios.get('http://localhost:5000/skull')
            .then(response => (this.msg = response.data))
            .catch(error => console.log(error))
        },
        
        gotohome(){
            this.$router.push('/')
        }
    },
    created()
    {
        this.getSkull()
    }
    
}
</script>
<style scoped>
h1 {
padding:10%;
  text-align: center;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
p{
    padding:10%;
    text-align: center;
}
div.btn{
    padding: 10%;
    
    margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}
</style>