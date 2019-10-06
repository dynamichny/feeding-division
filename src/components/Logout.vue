<template>
  <button @click.prevent="logout()">Logout</button>
</template>

<script>
import firebase from 'firebase';
export default {
  name: 'Logout',
  methods: {
    logout(){
      firebase.auth().signOut()
      .then(result => {
        this.$emit('isLogged', false)
      });
    }
  },
};
</script>

<style scoped lang='scss'>
button{
  position: absolute;
  top: 15px;
  right: 4%;
  z-index: 99999999;
  background: #222222;
  color: white;
  font-size: 15px;
  border: 2px solid;
  border-radius: 15px;
  padding: 8px 12px;
}
</style>