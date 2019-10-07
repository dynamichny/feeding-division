<template>
  <div class="groupjoinWrapper">
    <form @submit.prevent="handleRequest()">
      <input type="text" class="input" name="name" v-model="name" placeholder="Enter group name">
      <input type="submit" class="submit" value="Send request">
    </form>
  </div>
</template>

<script>
import db from './firebaseInit.js';

export default {
  name: 'JoinGroup',
  props: ['user'],
  data(){
    return{
      name: '',
    }
  },
  methods: {
    handleRequest(){
      db.collection('groups').doc(this.name).get()
      .then(result =>{
        if(result.exists) {
          let requests = result.data().requests;
          if(requests.findIndex(request => request.uid == this.user.uid) == -1){
            requests.push({ username: this.user.email, uid: this.user.uid });
            db.collection('groups').doc(this.name).update({
              requests
            });
            alert(`Request sent to: ${this.name}`)
          } 
        } else {
          alert("Group with this name does'nt exist.");
        }
      });
    }
  },
};
</script>

<style scoped lang='scss'>
.groupjoinWrapper{
  width: 100vw;
}
form{
  width: 85%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.input{
  border: none;
  border-bottom: 2px solid black;
  padding: 15px;
  outline: none;
  margin: 30px auto 30px;
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
}
</style>