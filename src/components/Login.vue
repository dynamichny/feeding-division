<template>
  <div class="loginWrapper">
    <form @submit.prevent="$emit('login', {email, password})">
      <div class="inputs">
        <div class="email-input">
          <label for="email">Email</label>
          <input type="email" name="email" v-model="email" required placeholder="Email">
        </div>
        <div class="password-input">
          <label for="password">Password</label>
          <input type="password" name="password" v-model="password" required placeholder="Password">
        </div>    
      </div>
      <button class="submit" type="submit">Log in</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data(){
    return{
      email: '',
      password: '',
    }
  }
};
</script>

<style scoped lang='scss'>
  .loginWrapper{
    position: absolute;
    left: 0;
    right: 0;
    margin: auto;
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
    justify-content: space-between;
    div{
      margin: auto;
      width: 90%;
      display: flex;
      justify-content: space-between;
      flex-direction: column;
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