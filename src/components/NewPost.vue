<template>
    <button @click.prevent="commitFeeding()" class="btn">Feed</button>
</template>

<script>
import db from './firebaseInit.js';
export default {
  name: 'NewPost',
  props: ['groupName', 'user'],
  methods:{
    commitFeeding(){
      db.collection('groups').doc(this.groupName).get()
      .then(doc =>{
        const posts = doc.data().posts;
        const date = new Date();
        posts.unshift({date, uid: this.user.uid, username: this.user.email});
        db.collection('groups').doc(this.groupName).update({posts});
      });
    }
  }
};
</script>

<style scoped lang='scss'>
.btn{
  position: absolute;
  bottom: 35px;
  left: 0;
  right: 0;
  margin: auto;
  width: 90%;
  height: 50px;
  padding: 5px;
  background: #ECBE45;
  color: white;
  border: none;
  font-size: 18px;
  font-weight: bold;
  text-shadow: 2px 2px 0px black;
  box-shadow: 2px 2px 0px black;
  cursor: pointer;
  outline: none;
  box-sizing: border-box;
}
</style>