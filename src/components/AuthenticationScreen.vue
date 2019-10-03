<template>
  <div class="enteringScreen-wrapper">
    <div class="btn login" @click="isLogin = true">Login</div>
    <div class="btn register" @click="isRegister = true">Register</div>

    <Login v-if="isLogin" @close="isLogin = false" @login="loginData = $event"/>
    <Register v-if="isRegister" @close="isRegister = false" @register="registerData = $event"/>
  </div>
</template>

<script>
import Login from './Login.vue'
import Register from './Register.vue'
import firebase from 'firebase';

export default {
  name: 'AuthenticationScreen',
  components:{
    Login,
    Register,
  },
  data(){
    return{
      isLogin: false,
      isRegister: false,
      user: null,
      loginData: null,
      registerData: null,
    }
  },
  watch:{
    loginData(){
      firebase.auth().signInWithEmailAndPassword(this.loginData.email, this.loginData.password)
        .then(result=>{
          this.user = result.user;
        }).catch(error => alert(error));
    },
    registerData(){
      firebase.auth().createUserWithEmailAndPassword(this.registerData.email, this.registerData.password)
        .catch(error => alert(error))
    },
    user(){
      this.$emit('user', this.user)
    }
  },
};
</script>

<style scoped lang='scss'>
.enteringScreen-wrapper{
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 80%;
}
.btn{
  border: 1px solid black;
  margin: 20px;
  text-align: center;
  padding: 25px 10px;
  cursor: pointer;
  font-size: 20px;
}
</style>