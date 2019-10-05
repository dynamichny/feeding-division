<template>
  <div class="groupWrapper">

    <div class="groupInfo">
      <h1>{{groupName}}</h1>
      <div class="icons">
        <div class="icon" v-for="(username, index) in usernames" :key="index">{{username}}</div> 
      </div>
      <button @click.prevent="isManage = !isManage" v-if="isAdmin" class="manage">{{manage}}</button>
      <GroupManage v-if="isManage" :groupData="groupData" :groupName="groupName" />
    </div>

    <Posts :groupData="groupData" :animal="animal" />
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
    NewPost,
  },
  props: ['groupName', 'user'],
  data(){
    return{
      groupData: null,
      isManage: false,
      isAdmin: false,
      animal: null,
      usernames: [],
    }
  },
  mounted(){
    db.collection('groups').doc(this.groupName).onSnapshot(doc =>{
          this.groupData = doc.data();
        });
    db.collection('groups').doc(this.groupName).get()
      .then(response =>{
        this.animal = response.data().animal;
        if(response.data().admin.uid == this.user.uid) {
          this.isAdmin = true;
          return;
        }
        this.isAdmin = false;
      })
  },
  watch: {
    groupData(){
      this.usernames = this.groupData.users.map(user => user.username[0].toUpperCase()).slice(0,5);
      this.usernames.unshift(this.groupData.admin.username[0].toUpperCase())
      }
  },
  computed: {
    manage(){
      return this.isManage ? 'Close' : 'Manage';
    }
  },
};
</script>

<style scoped lang='scss'>
.groupWrapper{
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;

}
.groupInfo{
  background: white;
  padding: 5%;
  overflow: auto;
  z-index: 100000;
  max-height: 40vh;
  box-shadow: 0px 1px 2px gray;
  transition: all .3s;
}
.manage{
  background: none;
  border: none;
  color: rgb(0, 122, 255);
  outline: none;
  font-size: 18px;
  float: right;
}
h1{
  margin: 0;
}
.icon{
  width: 30px;
  height: 30px;
  background: rgb(234, 151, 255);
  margin: 10px 3px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  box-shadow: 1px 1px 2px gray;
}
.icons{
  display: flex;
}
</style>