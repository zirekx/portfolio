<template>
<div>
<div id="wrapper">
 <div id='player_wrapper' >
  <video @loadedmetadata="videoDurationlog"
  @timeupdate="videoCurrentTime"
  @progress="checkProgress"
  
  ref = "videoPlayer"
  @mouseover="hover = true"
  @mouseout="hover = false"
  @click="toggle_vid"
  id='video_player'>
   <source src='@/assets/filmik.mp4' type='video/mp4'>
  </video>
  <div id='player_controls'
  v-show="hover"
  @mouseover="hover = true"
  @mouseout="hover = false"
  >

   <input v-if="isPlaying" type="image" 
   :src="pauseButtonImage" 
   @click="toggle_vid" id="play_button">
   <input v-else type="image" 
   :src="playButtonImage" 
   @click="toggle_vid" id="play_button">
   <span>{{currentVideoTime}}</span>
  <progress refs="progress" id="progress" max="100" :value="currentProgress"> Progress </progress>
  <span>{{videoDuration}}</span>
   <img v-if="muted" :src='mutedImage' id="vol_img" @click="mute_video">
   <img v-else :src='soundImage' id="vol_img" @click="mute_video">
   <input ref = "volumePlayer" type="range" id="change_vol" v-on:change="change_volume" step="0.05" min="0" max="1"  v-model="volumeValue">
   <input
    @click="toggle_fullscreen"
    type="image" :src="fulscreenImage" id="fullscreen">
  </div>
 </div>
</div>
  </div>

</template>

<script>

export default {
  name: 'VideoPlayer',
  data() {
    return{
      isPlaying: false,
      playButtonImage: require('@/assets/play.png'),
      pauseButtonImage: require('@/assets/pause.png'),
      soundImage: require('@/assets/sound.png'),
      mutedImage: require('@/assets/muted.png'),
      fulscreenImage:require('@/assets/fullscreen.png'),
      closefullscreenImage:require('@/assets/closefullscreen.png'),
      currentVideoTime: "00:00",
      videoDuration:"",
      currentProgress:0,
      muted:0,
      volumeValue:1,
      hover:false,
      
     
      
    }
  },
  methods: {
    toggle_vid(){
      if(this.$refs.videoPlayer.paused)
       {
        this.isPlaying = true;
        this.$refs.videoPlayer.play();
       }
       else{
        this.$refs.videoPlayer.pause();
        this.isPlaying = false;
       }

    },
    change_volume(){
      this.$refs.videoPlayer.volume= this.volumeValue;
      if(this.$refs.videoPlayer.volume==0){
        this.muted=1;

      }
      else{
        this.muted=0;
      }
    },
    stop_video(){
      this.$refs.videoPlayer.pause();
      this.currentTime = 0;

    },
    mute_video(){
      if(this.muted==0){
        this.$refs.videoPlayer.volume=0;
        this.muted=1;
      }
      else{
        this.$refs.videoPlayer.volume=this.volumeValue;
        this.muted=0;
      }
    },
    toggle_fullscreen(){
      this.$refs.videoPlayer.webkitRequestFullScreen();
    },
    time_format(num){
      let hours= Math.floor(num / 3600);
      let minutes = Math.floor((num % 3600)/60);
      let seconds = Math.floor(num % 60);

      hours = hours < 10 ? "0" + hours : hours;
      minutes = minutes < 10 ? "0" + minutes : minutes;
      seconds = seconds < 10 ? "0" + seconds : seconds;

      if(hours>0){
        return  hours+':'+minutes+":"+seconds
      }
      else{
        return  minutes+":"+seconds;
        
      }
    },
    videoDurationlog(){
      return this.videoDuration = this.time_format(this.$refs.videoPlayer.duration);
    
    },
    videoCurrentTime(){
      return this.currentVideoTime = this.time_format(this.$refs.videoPlayer.currentTime);
    },
    checkProgress(){
      //return this.currentProgress = Math.floor((this.currentVideoTime/this.videoDuration)*100);
     //return this.currentProgress = Math.floor((this.$refs.videoPlayer.currentTime/this.$refs.videoPlayer.duration)*100);
    }

    
    
  },
  mounted(){
    console.log(this.$refs.videoPlayer.currentTime);
    console.log(this.$refs.videoPlayer.duration);
    console.log(Math.floor((this.$refs.videoPlayer.currentTime/this.$refs.videoPlayer.duration)*100));
      
      
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$primary-color: #2cbc63;
.toggle-fullscreen{
  width:100vw;
  height:100vh;
}
#wrapper
{
 text-align:center;
 margin:0 auto;
 padding:0px;
 width:100%;
}
#player_wrapper
{
 position:relative;
 width:400px;
 margin-left:300px;
}
#video_player
{
 width:1000px;
}

#player_controls
{
  opacity: 0.8;
   // transition: opacity 1s 2s;
 top:93%;
 position:absolute;
 background: rgba(0, 0, 0, 0.5);
 width:1000px;
 padding:7px;
 box-sizing:border-box;

}
#video_player:hover + #player_controls{
 //opacity: 1;
    //transition: opacity 0.5s 0s;
}
input[type="image"]
{
 float:left;
 height:20px;
 margin-left:2px;
 margin-right:5px;
 margin-top:2px;
}
#vol_img
{
 margin-left:150px;
 width:20px;
}
.change_vol{
  float:right;
}
input[type="range"]{
  accent-color:$primary-color;
  
}
#fullscreen{
  position:absolute;
  left:96%;
}

  span{
    color:White;
    padding-left:5px;
    padding-right:5px;
  }


</style>
