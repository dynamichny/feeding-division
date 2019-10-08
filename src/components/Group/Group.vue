<template>
  <div class="groupWrapper">
    <div class="groupInfo" ref="info">
      <h1>{{groupName}}</h1>
      <div class="icons">
        <div
          class="icon"
          v-for="(username, index) in usernames"
          :key="index"
          :style="{ backgroundColor: letterColor(username)}"
        >{{username}}</div>
      </div>
      <button @click.prevent="isManage = !isManage" class="manage">{{manage}}</button>
      <GroupManage
        v-if="isManage"
        :groupData="groupData"
        :groupName="groupName"
        :currentUser="currentUser"
        :isAdmin="isAdmin"
      />
    </div>

    <Posts :groupData="groupData" :animal="animal" :groupInfo="$refs.info" />
    <NewPost :groupName="groupName" :currentUser="currentUser" />
  </div>
</template>

<script>
import db from "../firebaseInit.js";
import GroupManage from "./GroupManage.vue";
import Posts from "./Posts.vue";
import NewPost from "./NewPost.vue";

export default {
  name: "Group",
  components: {
    GroupManage,
    Posts,
    NewPost
  },
  props: ["groupName", "currentUser"],
  data() {
    return {
      groupData: null,
      isManage: false,
      isAdmin: false,
      animal: null,
      usernames: []
    };
  },
  mounted() {
    db.collection("groups")
      .doc(this.groupName)
      .onSnapshot(doc => {
        this.groupData = doc.data();
      });
    db.collection("groups")
      .doc(this.groupName)
      .get()
      .then(response => {
        this.animal = response.data().animal;
        if (response.data().admin.uid == this.currentUser.uid) {
          this.isAdmin = true;
          return;
        }
        this.isAdmin = false;
      });
  },
  watch: {
    groupData() {
      this.usernames = this.groupData.users
        .map(user => user.username[0].toUpperCase())
        .slice(0, 5);
      this.usernames.unshift(this.groupData.admin.username[0].toUpperCase());
    }
  },
  computed: {
    manage() {
      return this.isManage ? "Close" : "Manage";
    }
  },
  methods: {
    letterColor(user) {
      return `hsl(${(user.charCodeAt(0) * 3649149) % 360}, 50%, 50%)`;
    }
  }
};
</script>

<style scoped lang='scss'>
.groupWrapper {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  display: flex;
  flex-direction: column;
}
.groupInfo {
  background: white;
  padding: 15px 5%;
  overflow: auto;
  z-index: 999999;
  max-height: 40vh;
  box-shadow: 0px 1px 2px gray;
  transition: all 0.3s;
}
.manage {
  background: none;
  border: none;
  color: rgb(0, 122, 255);
  outline: none;
  font-size: 18px;
  float: right;
  margin: 0 0;
  cursor: pointer;
}
h1 {
  margin: 0 calc(60px + 4%) 0 0;
}
.icon {
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
.icons {
  display: flex;
}
</style>