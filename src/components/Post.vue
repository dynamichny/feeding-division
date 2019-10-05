<template>
  <div class='postWrapper'>
    <div class="icon">{{username[0].toUpperCase()}}</div>
    <p class="text">{{animal}} was fed <span class="time">{{time}}</span> ago by <span class="name">{{username}}</span>.</p>
    
  </div>
</template>

<script>
export default {
  name: 'Post',
  props: ['content', 'animal'],
  data(){
    return{
      timeNow: null,
      setTime: '',
    }
  },
  computed: {
    time(){
      let timeNow = this.timeNow;
      let seconds_num = -(this.content.date.seconds - Math.round(timeNow/1000));
      let hours = Math.floor(seconds_num / 3600);
      let minutes = Math.floor((seconds_num - (hours * 3600)) / 60);
      let seconds = seconds_num - (hours * 3600) - (minutes * 60);
      return  hours > 0 ? `${hours}h ${minutes}m` : minutes > 0 ? `${minutes}min` : `${seconds}s`;
    },
    username(){
      return this.content.username.match(/[a-zA-Z0-9_.+-]+/g)[0];
    }
  },
  mounted(){
    this.timeNow = Date.now();
    this.setTime = setInterval(()=>{
      this.timeNow = Date.now()
    },1000);
  },
  beforeDestroy(){
    clearInterval(this.setTime);
  }
};
</script>

<style scoped lang='scss'>
.postWrapper{
  display: flex;
  align-items: center;
  justify-content: end;
  margin: 20px 5%;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 3px 3px 10px rgb(184, 184, 184);
  overflow: auto;
  max-height: 60vh;
}
p{
  margin: 0 0 0 10px;
}
.name{
  color: #ECBE45;
}
.time{
  font-weight: bold;
}
.icon{
  padding: 8px 12px;
  background: rgb(151, 200, 255);
  margin: 10px auto;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  box-shadow: 1px 1px 2px gray;

}
</style>