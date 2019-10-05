<template>
  <div class="selgroupWrapper">
    <div class="btn create" :class="isCreateGroup ? 'btn-yellow': ''" @click="isCreateGroup = !isCreateGroup; isJoinGroup = false">Create new group</div>
    <GroupCreate v-if="isCreateGroup" :user="user"/>

    <div class="btn join" :class="isJoinGroup ? 'btn-yellow': ''" @click="isJoinGroup = !isJoinGroup; isCreateGroup = false">Send a request to join</div>
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
  background: url(https://i.gifer.com/1L7Q.gif);
  background-position: center center;
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}
.btn{
  background: rgb(34, 34, 34);
  color: white;
  margin: 0 5% 10px;
  text-align: center;
  padding: 15px 10px;
  cursor: pointer;
  font-size: 20px;
}
.btn-yellow{
  background: #ECBE45;
}


</style>