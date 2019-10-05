<template>
  <div id="app">
    <AuthenticationScreen v-if="!isLogged" @user="user = $event; isLogged = true"/>
    <SelectGroup v-if="!group && isLogged" :user="user" />
    <Group v-if="group && isLogged" :groupName="group" :user="user"/>
  </div>
</template>

<script>
import AuthenticationScreen from './components/AuthenticationScreen.vue';
import SelectGroup from './components/SelectGroup.vue';
import Group from './components/Group.vue';
import db from './components/firebaseInit.js';

export default {
  name: 'app',
  components: {
    AuthenticationScreen,
    SelectGroup,
    Group,
  },
  data(){
    return{
      isLogged: false,
      user: null,
      group: false,
    }
  },
  watch: {
    isLogged(){
      db.collection('users').doc(this.user.uid).onSnapshot(doc =>{
          if(doc.exists){
            this.group = doc.data().group;
          } else {
            this.group = false;
          }
        });
    }
  },
}
</script>

<style lang="scss">
body{
  margin: 0;
  padding: 0;
  background: white;

}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #000000;
  width: 100%;
  height: 100vh;
}
.switch-enter-active, .switch-leave-active{
  transition: all 0.2s ease-in-out;
}
.switch-enter{
  transform: translateX(100%);
}
.switch-leave-to{
  transform: translateX(-100%);
}
</style>
