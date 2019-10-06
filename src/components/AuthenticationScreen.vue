<template>
  <div class="enteringScreen-wrapper">
    <h1>Feeding Division</h1>
    <transition name="switch">
      <Login  @login="loginData = $event" v-if="isLogin"/>
    </transition>
    <transition name="switch">
      <Register v-if="isRegister"  @register="registerData = $event"/>
    </transition>
    <div class="btn" @click="isRegister = !isRegister; isLogin = !isLogin">Switch to {{goTo}}</div>
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
      isLogin: true,
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
      if(!!this.registerData.password){
        firebase.auth().createUserWithEmailAndPassword(this.registerData.email, this.registerData.password)
          .then(()=>{
            firebase.auth().signInWithEmailAndPassword(this.registerData.email, this.registerData.password)
            .then(result=>{
              this.user = result.user;
            });
          })
          .catch(error => alert(error))
      } else{
        alert("Passwords aren't the same.")
      }
    },
    user(){
      this.$emit('user', this.user)
    }
  },
  computed:{
    goTo(){
      return this.isLogin  ? 'Register' : 'Login';
    }
  },
};
</script>

<style scoped lang='scss'>
.enteringScreen-wrapper{
  background: url(https://i.gifer.com/1L7Q.gif);
  background-position: center center;
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
}
.btn{
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgb(34, 34, 34);
  color: white;
  margin: 0 0 0;
  text-align: center;
  padding: 15px 10px;
  cursor: pointer;
  font-size: 20px;
}
h1{
  position: absolute;
  top: 30px;
  left: 0;
  right: 0;
  text-align: center;
  color: white;
  text-shadow: 2px 0px 1px black;
}
.switch-enter-active, .switch-leave-active{
  transition: all 0.5s ease-in-out;
}
.switch-enter{
  transform: translateX(100%);
  opacity: 0;
}
.switch-leave-to{
  transform: translateX(-100%);
  opacity: 0;
}

</style>