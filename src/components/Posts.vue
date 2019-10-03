<template>
  <div>
    <div class="post" v-for="(post, index) in posts" :key="index">
      {{post.date.seconds}}
    </div>
  </div>
</template>

<script>
import db from './firebaseInit.js';
export default {
  name: 'Posts',
  props: ['groupData'],
  computed: {
    posts(){
      while(this.groupData) return this.groupData.posts.slice(0,4);
    },
    times(){
      let posts = this.posts;
      console.log(posts)
    }
  },
};
</script>

<style scoped lang='scss'>

</style>