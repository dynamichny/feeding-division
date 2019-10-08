<template>
  <div class="selgroupWrapper">
    <div
      class="btn create"
      :class="isCreateGroup ? 'btn-yellow': ''"
      @click="isCreateGroup = !isCreateGroup; isGroupRequest = false"
    >Create new group</div>
    <GroupCreate v-if="isCreateGroup" :currentUser="currentUser" />

    <div
      class="btn join"
      :class="isGroupRequest ? 'btn-yellow': ''"
      @click="isGroupRequest = !isGroupRequest; isCreateGroup = false"
    >Send a request to join</div>
    <GroupRequest v-if="isGroupRequest" :currentUser="currentUser" />
  </div>
</template>

<script>
import GroupCreate from "./GroupCreate.vue";
import GroupRequest from "./GroupRequest.vue";

export default {
  name: "SelectGroup",
  components: {
    GroupCreate,
    GroupRequest
  },
  props: ["currentUser"],
  data() {
    return {
      isCreateGroup: false,
      isGroupRequest: false
    };
  }
};
</script>

<style scoped lang='scss'>
.selgroupWrapper {
  height: 100%;
  background: url(https://i.gifer.com/1L7Q.gif);
  background-position: center center;
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}
.btn {
  background: rgb(34, 34, 34);
  color: white;
  margin: 0 5% 10px;
  text-align: center;
  padding: 15px 10px;
  cursor: pointer;
  font-size: 20px;
}
.btn-yellow {
  background: #ecbe45;
}
</style>