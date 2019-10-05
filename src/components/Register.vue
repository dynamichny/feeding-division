<template>
  <div class="registerWrapper">
    <form @submit.prevent="sendForm()">
      <div class="inputs">
        <div class="email">
          <label for="email">Email</label>
          <input type="email" name="email" v-model="email" required placeholder="Email">
        </div>
        <div class="password">
          <label for="password">Password</label>
          <input type="password" name="password" v-model="pass" required placeholder="Password">
        </div>
        <div class="re-password">
          <label for="password">Re-Password</label>
          <input type="password" name="password" v-model="repass" required placeholder="Re-write password">
        </div>
      </div>
      <button class="submit" type="submit">Register</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Register',
  data(){
    return{
      email: '',
      pass: '',
      repass: '',
    }
  },
  methods: {
    sendForm(){
      let password = this.pass === this.repass ? this.pass : false;
      this.$emit('register', {email: this.email, password})
    }
  },
};
</script>

<style scoped lang='scss'>
  .registerWrapper{
    width: 100%;
    margin:  0 auto;
    color: black;
  }
  
  form{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
  }
  .inputs{
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    div{
      margin: auto;
      width: 90%;
      display: flex;
      justify-content: space-between;
      flex-direction: column;
      }
      input{
        border: none;
        border-bottom: 2px solid black;
        padding: 15px;
        outline: none;
        margin: 0 0 30px;
        color: black;
        background: white;
        width: 100%;
        font-size: 18px;
        transition: all 0.15s;
        box-sizing: border-box;
        &:focus {
          border-bottom: 5px solid black;
          box-shadow: 0px 10px 15px -15px black;
        }
      }
      label{
        margin: 0;
        color: white;
      }
    }
.submit{
  width: 90%;
  height: 50px;
  padding: 5px;
  background: black;
  color: white;
  border: none;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  outline: none;
  box-sizing: border-box;
  margin: 0 0 20px;
}
</style>