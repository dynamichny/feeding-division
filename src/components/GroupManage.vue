<template>
  <div class="manageWrappeer">
    <div v-if="users.length > 0 && isAdmin" class="users" >
      <p>Users</p>
      <div class="user" v-for="user in users" :key="user.uid">
        {{user.username}}
        <button class="remove" @click.prevent="removeUser(user)">Remove</button>
      </div>
    </div>
    <div v-if="requests.length > 0 && isAdmin" class="requests">
      <p>Requests</p>
      <div class="request" v-for="user in requests" :key="user.uid">
        {{user.username}}
        <div class='buttons'>
          <button class="accept" @click.prevent="acceptUser(user)">Accept</button>
          <button class="remove" @click.prevent="discardUser(user)">Discard</button>
        </div>
      </div>
    </div>
    <div class="exit">
      <button @click.prevent="exitGroup()">{{exit}}</button>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit.js';
export default {
  name: 'GroupManage',
  props: ['groupData', 'groupName', 'currentUser', 'isAdmin'],
  computed: {
    requests(){
      return this.groupData.requests;
    },
    users(){
      return this.groupData.users;
    },
    exit(){
      return this.isAdmin ? 'Delete group' : 'Exit group';
    }
  },
  methods: {
    acceptUser(user){
      db.collection('users').doc(user.uid).get()
        .then(result1 => {
          db.collection('groups').doc(this.groupName).get()
          .then(result2 => {
            if(!result1.exists){
              const users = result2.data().users;
              users.push(user);
              db.collection('groups').doc(this.groupName).update({users});
            
              db.collection('users').doc(user.uid).set({
                username: user.username,
                group: this.groupName,
              });
            }

            const requests = result2.data().users;
            requests.splice(requests.findIndex(request => request.uid == user.uid), 1);
            db.collection('groups').doc(this.groupName).update({requests});
          });
        })
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
    },
    exitGroup(){
      if(!this.isAdmin){
        db.collection('groups').doc(this.groupName).get()
          .then(result => {
            let users = result.data().users;
            users.splice(users.findIndex(user => user.uid === this.currentUser.uid), 1);
            db.collection('groups').doc(this.groupName).update({users});
            db.collection('users').doc(this.currentUser.uid).delete();
          });
      } else {
        db.collection('groups').doc(this.groupName).get()
        .then(result => {
          let users = result.data().users;
          users.map(user => {
            db.collection('users').doc(user.uid).delete();
          });
          db.collection('users').doc(this.currentUser.uid).delete();
          db.collection('groups').doc(this.groupName).delete();
        });
      }
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
  cursor: pointer;
}
.accept{
  background: #2ECC71;
  color: white;
  border: 2px solid white;
  border-radius: 15px;
  padding: 7px 12px;
  cursor: pointer;
}
.exit{
  button{
    margin: 25px 0 0;
    width: 100%;
    background: #E74C3C;
    color: white;
    border: 2px solid white;
    border-radius: 15px;
    padding: 8px 12px;
    cursor: pointer;
  }
}
</style>