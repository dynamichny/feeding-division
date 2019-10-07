<template>
  <div class="groupcreateWrapper">
    <form @submit.prevent="createGroup()">
      <input type="text" class="input" name="groupName" v-model="name" placeholder="Group name">
      <input type="text" class="input" name="petName" v-model="animal" placeholder="Animal">
      <input type="submit" class="submit" value="Create">
    </form>
  </div>
</template>

<script>
import db from './firebaseInit.js';
import firebase from 'firebase';

export default {
  name: 'GroupCreate',
  props: ['currentUser'],
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
              username: this.currentUser.email, uid: this.currentUser.uid
              },
            users: [],
            requests: [],
            posts: [],
          }).then(()=>{
            db.collection('users').doc(this.currentUser.uid).set({
              group: this.name,
              username: this.currentUser.email,
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
  width: 100vw;
}
form{
  height: 100%;
  width: 85%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.input{
  border: none;
  border-bottom: 2px solid black;
  padding: 15px;
  outline: none;
  margin: 10px auto 10px;
  color: black;
  background: white;
  width: 100%;
  font-size: 18px;
  transition: all 0.15s;
  box-sizing: border-box;
  &:focus {
    border-bottom: 5px solid black;
    box-shadow: 0px 10px 15px -15px black;
  }
}
.submit{
  width: 100%;
  height: 50px;
  padding: 5px;
  background: black;
  color: white;
  border: none;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  outline: none;
  box-sizing: border-box;
  margin: 0 0 20px;
}
</style>