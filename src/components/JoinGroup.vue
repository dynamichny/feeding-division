<template>
  <div class="groupjoinWrapper">
    <form @submit.prevent="handleRequest()">
      <div>
        <label for="name">Enter group name</label>
        <input type="text" name="name" v-model="name">
      </div>
      <input type="submit" value="Send request">
    </form>
  </div>
</template>

<script>
import db from './firebaseInit.js';
import has from 'lodash.has'

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
  div{
    input{
      width: 100%;
    }
  }
  input{
    margin: 0 0 35px; 
  }
}
</style>