<template>
  <div class="groupcreateWrapper">
    <form @submit.prevent="createGroup()">
      <label for="groupName" >Group name</label>
      <input type="text" name="groupName" v-model="name">
      <label for="petName">Animal</label>
      <input type="text" name="petName" v-model="animal">
      <input type="submit" value="Create">
    </form>
  </div>
</template>

<script>
import db from './firebaseInit.js';
import firebase from 'firebase';

export default {
  name: 'GroupCreate',
  props: ['user'],
  data(){
    return{
      name: null,
      animal: null,
      groupCreated: false,
    }
  },
  methods:{
    createGroup(){
      db.collection('groups').doc(this.name).get().then(response => {
        if(!response.exists){
          db.collection('groups').doc(this.name).set({
            animal: this.animal,
            admin: {
              username: this.user.email, uid: this.user.uid
              },
            users: [],
            requests: [],
            posts: [],
          }).then(()=>{
            db.collection('users').doc(this.user.uid).set({
              group: this.name,
              username: this.user.email,
            }).then(()=> this.groupCreated = true);
          });
        } else {
          alert('Group with that name already exist.')
        }
      });
      
    }
  }
};
</script>

<style scoped lang='scss'>
.groupcreateWrapper{
  position: fixed;
  top:0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100vw;
  height: 100vh;
  background: white;
}
form{
  height: 100%;
  width: 75%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  input{
    margin: 0 0 35px; 
  }
}
</style>