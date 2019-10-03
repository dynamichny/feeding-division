<template>
  <div class="selgroupWrapper">
    <div class="btn create" @click="isCreateGroup = true">Create new group</div>
    <div class="btn join" @click="isJoinGroup = true">Send a request to join</div>
    <GroupCreate v-if="isCreateGroup" :user="user"/>
    <JoinGroup v-if="isJoinGroup" :user="user"/>
  </div>
</template>

<script>
import GroupCreate from './GroupCreate.vue'
import JoinGroup from './JoinGroup.vue'

export default {
  name: 'SelectGroup',
  components: {
    GroupCreate,
    JoinGroup,
  },
  props: ['user'],
  data(){
    return{
      isCreateGroup: false,
      isJoinGroup: false,   
    }
  }
};
</script>

<style scoped lang='scss'>
.selgroupWrapper{
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}
.btn{
  border: 1px solid black;
  margin: 20px;
  text-align: center;
  padding: 25px 10px;
  cursor: pointer;
  font-size: 20px;
}

</style>