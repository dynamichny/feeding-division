<template>
    <button @click.prevent="commitFeeding()">Feed</button>
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

</style>