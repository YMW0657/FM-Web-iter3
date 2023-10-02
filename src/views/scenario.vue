<template>
  <div class="video">
    <div class="video-container">
      <div class="video-sup">This is a scenario simulation. After each video finishes playing, you can decide the next step. Be aware that the scenario might change based on your choices, so please choose carefully.</div>
      <video controls autoplay ref="videoElement">
          <source :src="videoSource" type="video/mp4">
      </video>
      <div class="question-wrapper" v-if="questionShow">
        <div class="question-header">
          WHAT'S YOUR NEXT DO?
        </div>
        <div class="question-lists">
          <div class="question-list" @click="toJump('Y')">{{btns[route.query.id]['Y']}}</div>
          <div class="question-list" @click="toJump('N')">{{btns[route.query.id]['N']}}</div>
        </div>

      </div>
    </div>
  </div>
</template>
<script setup>
  import { onMounted, ref  } from 'vue';
  import { useRoute } from "vue-router";
  const questionShow = ref(false);
  const route = useRoute();
  const videoId = route.query.id
  const videoSource = ref();
  videoSource.value = 'src/assets/video/'+videoId+'.mp4';
  const videoElement = ref();
  const videoIdSource = ref({
    1: {
      Y: 2,
      N: 3
    },
    2: {
      Y:4,
      N:5
    },
    3: {
      Y:6,
      N:7
    }
  })
  const btns = ref({
    1:{
      Y:"Open",
      N:"Close"
    },
    2:{
      Y:"Anwser",
      N:"Close"
    },
    3:{
      Y:"Enter Info",
      N:"Close"
    },
    4:{
      Y:"Confirm",
      N:"Ignore"
    },
    5:{
      Y:"Check",
      N:"Ignore"
    }
  })
  const formatSourceFn=(source, videoId, type)=>{
    if(source.value[videoId]){
      return source.value[videoId][type];
    }else{
      return ""
    }
  };
  const toJump = (type)=>{
    const nextVideoId = formatSourceFn(videoIdSource, videoId, type);
    window.location.href= "/scenario?id="+nextVideoId
  }
  onMounted(() => {
  const actualVideoElement = videoElement.value;
  console.log(actualVideoElement);
  if (actualVideoElement) {
    actualVideoElement.load();
    // actualVideoElement.addEventListener('loadedmetadata', () => {
    //   const currentTime = actualVideoElement.currentTime;
    // });
    actualVideoElement.addEventListener('timeupdate', () => {
      const totalDuration = actualVideoElement.duration;
      const currentTime = actualVideoElement.currentTime;
      console.log(totalDuration - currentTime);

      if (totalDuration - currentTime <= 2.5) {
        if (!questionShow.value) {
          questionShow.value = true;
        }
      }
    });
  }
});
</script>
<style scoped>
  .video{
    padding-top: 71px;
    height: 100vh;
  }
  .video-container{
    position: relative;
    width: 70%;
    margin: 50px auto;
    padding: 20px;
    background-color: rgba(255, 255, 255, 0.7);
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  }
  video {
    width: 100%;
    height: auto;
  }
  .question-wrapper{
    position: absolute;
    left:0;
    top:0;
    right:0;
    bottom:0;
    background: rgba(0, 0, 0, 0.5);
    z-index: 9;
    transition: all 1s ease;
    margin: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    /* margin-top: -8rem; */
    border-radius: 10px;
  }
  .question-header{
    text-align: center;
    color: #fff;
    font-family: "Montserrat", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
    font-size: 3.5rem;
  }
  .question-lists{
    width:100%;
    display: flex;
    margin-top: 80px;
    justify-content: center;
  }
  .question-list{
    width:200px;
    height: 200px;
    background: #ffd84c99;
    border-radius: 4px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    font-size: 2rem;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    cursor: pointer;
    transition: background-color 0.2s ease;
    text-transform: uppercase;
    letter-spacing: 1px;
  }
  .question-list:nth-of-type(1){
    margin-right:20rem;
  }
  .question-list:hover{
    background: #dfbd4599;
  }
  .video-sup{
    text-align: left;
    font-family: "Montserrat", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
    font-size:1rem;
    font-weight: bold;
  }
</style>