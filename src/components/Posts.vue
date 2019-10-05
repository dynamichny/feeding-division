<template>
  <div class="postsWrapper">
    <Post v-for="(post, index) in posts" :key="index" :content="post" :animal="animal" />
  </div>
</template>

<script>
import db from './firebaseInit.js';
import Post from './Post.vue';

export default {
  name: 'Posts',
  components:{
    Post
  },
  props: ['groupData', 'animal'],
  computed: {
    posts(){
      while(this.groupData) return this.groupData.posts.slice(0,3).reverse();
    },
    times(){
      let posts = this.posts;
      console.log(posts)
    }
  },
};
</script>

<style scoped lang='scss'>
.postsWrapper{
  position: absolute;
  left: 0;
  right: 0;
  bottom: 100px;
  z-index: -1;
}
</style>