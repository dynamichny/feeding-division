<template>
  <div>
    <button @click.prevent="isManage = !isManage" v-if="isAdmin">Manage</button>
    <GroupManage v-if="isManage" :groupData="groupData" :groupName="groupName" />
    <Posts :groupData="groupData" />
    <NewPost :groupName="groupName" :user="user" />
  </div>
</template>

<script>
import db from './firebaseInit.js';
import GroupManage from './GroupManage.vue';
import Posts from './Posts.vue';
import NewPost from './NewPost.vue';

export default {
  name: 'Group',
  components: {
    GroupManage,
    Posts,
    NewPost
  },
  props: ['groupName', 'user'],
  data(){
    return{
      groupData: null,
      isManage: false,
      isAdmin: false,
    }
  },
  mounted(){
    db.collection('groups').doc(this.groupName).onSnapshot(doc =>{
          this.groupData = doc.data();
        });
    db.collection('groups').doc(this.groupName).get()
      .then(response =>{
        if(response.data().admin.uid == this.user.uid) {
          this.isAdmin = true;
          return;
        }
        this.isAdmin = false;
      })
  },
};
</script>

<style scoped lang='scss'>

</style>