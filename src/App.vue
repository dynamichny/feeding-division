<template>
  <div id="app">
    <AuthenticationScreen v-if="!isLogged" @currentUser="currentUser = $event; isLogged = true" />
    <Logout v-if="isLogged" @isLogged="isLogged = $event" />
    <SelectGroup v-if="!groupName && isLogged" :currentUser="currentUser" />
    <Group v-if="groupName && isLogged" :groupName="groupName" :currentUser="currentUser" />
  </div>
</template>

<script>
import AuthenticationScreen from "./components/Authentication/AuthenticationScreen.vue";
import SelectGroup from "./components/GroupSelect/SelectGroup.vue";
import Logout from "./components/Authentication/Logout.vue";
import Group from "./components/Group/Group.vue";
import db from "./components/firebaseInit.js";

export default {
  name: "app",
  components: {
    AuthenticationScreen,
    SelectGroup,
    Group,
    Logout
  },
  data() {
    return {
      isLogged: false,
      currentUser: null,
      groupName: false
    };
  },
  watch: {
    isLogged() {
      if (this.isLogged) {
        db.collection("users")
          .doc(this.currentUser.uid)
          .onSnapshot(doc => {
            if (doc.exists) {
              this.groupName = doc.data().group;
            } else {
              this.groupName = false;
            }
          });
      }
    }
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
  background: white;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: #000000;
  width: 100%;
  height: 100vh;
}
.switch-enter-active,
.switch-leave-active {
  transition: all 0.2s ease-in-out;
}
.switch-enter {
  transform: translateX(100%);
}
.switch-leave-to {
  transform: translateX(-100%);
}
</style>
