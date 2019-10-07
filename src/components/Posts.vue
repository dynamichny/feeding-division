<template>
  <div>
    <div class="postsWrapper" ref="wrapper" :style="{ top: infoHeight }">
      <Post v-for="(post, index) in posts" :key="index" :content="post" :animal="animal" />
    </div>
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
  props: ['groupData', 'animal', 'groupInfo'],
  computed: {
    posts(){
      while(this.groupData) return this.groupData.posts.reverse();

    },
    infoHeight(){
      while(this.groupInfo) return `${this.groupInfo.offsetHeight}px`;
    }
  },
  updated(){
    let container = this.$refs.wrapper;
    container.scrollTop = container.scrollHeight;
  }
};
</script>

<style scoped lang='scss'>
.postsWrapper{
  position: absolute;
  left: 0;
  right: 0;
  bottom: 100px;
  height: auto;
  overflow: auto;
  z-index: 0;
  }
</style>