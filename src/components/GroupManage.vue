<template>
  <div class="manageWrappeer">
    <div v-if="users.length > 0" class="users">
      <p>Users</p>
      <div class="user" v-for="user in users" :key="user.uid">
        {{user.username}}
        <button class="remove" @click.prevent="removeUser(user)">Remove</button>
      </div>
    </div>
    <div v-if="requests.length > 0" class="requests">
      <p>Requests</p>
      <div class="request" v-for="user in requests" :key="user.uid">
        {{user.username}}
        <div class='buttons'>
          <button class="accept" @click.prevent="acceptUser(user)">Accept</button>
          <button class="remove" @click.prevent="discardUser(user)">Discard</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit.js';
import has from 'lodash.has'
export default {
  name: 'GroupManage',
  props: ['groupData', 'groupName'],
  computed: {
    requests(){
      return this.groupData.requests;
    },
    users(){
      return this.groupData.users;
    }
  },
  methods: {
    acceptUser(user){
      db.collection('groups').doc(this.groupName).get()
      .then(result => {
        const users = result.data().users;
        users.push(user);
        db.collection('groups').doc(this.groupName).update({users});
   
        const requests = result.data().users;
        requests.splice(requests.findIndex(request => request.uid == user.uid), 1);
        db.collection('groups').doc(this.groupName).update({requests});
        
        db.collection('users').doc(user.uid).set({
          username: user.username,
          group: this.groupName,
        });
      });
    },
    discardUser(user){
      db.collection('groups').doc(this.groupName).get()
      .then(result => {
        const requests = result.data().users;
        requests.splice(requests.findIndex(request => request.uid == user.uid), 1);
        db.collection('groups').doc(this.groupName).update({requests});
      });
    },
    removeUser(user){
      db.collection('groups').doc(this.groupName).get()
      .then(result => {
        const groupUsers = result.data().users;
        groupUsers.splice(groupUsers.findIndex(groupUser => groupUser.uid == user.uid), 1);
        db.collection('groups').doc(this.groupName).update({users: groupUsers});
        db.collection('users').doc(user.uid).delete();
      });
    }
  },
};
</script>

<style scoped lang='scss'>
.maanageWrapper{
  margin: 10px 0 0;
}
p{
  text-decoration: underline;
  margin: 0;
  font-size: 18px;
}
.users, .requests{
  margin: 20px 0 0;
}
.user{
  margin: 8px 0 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.request{
  margin: 8px 0 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.remove{
  background: #E74C3C;
  color: white;
  border: 2px solid white;
  border-radius: 15px;
  padding: 8px 12px;
}
.accept{
  background: #2ECC71;
  color: white;
  border: 2px solid white;
  border-radius: 15px;
  padding: 7px 12px;
}
</style>