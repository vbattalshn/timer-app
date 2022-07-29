<template>
  <div id="app" class="page-hero" v-auto-animate>
    <h2 class="time">{{ calcedTime }}</h2>
    <ul v-if="marked.length" class="marked" tabindex="0" v-auto-animate>
      <li v-for="mark in marked" :key="mark.id">
        <span class="turn">{{ mark.id + 1 }}</span>
        <span class="markTime">{{ calcTime(mark.time) }}</span>
      </li>
    </ul>
    <div class="buttons" v-auto-animate="{ duration: 100 }">
      <button v-if="isPause == false && isPlay == false || isPause == true && isPlay == true" @click="play" class="play btn">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          viewBox="0 0 16 16"
        >
          <path
            d="m11.596 8.697-6.363 3.692c-.54.313-1.233-.066-1.233-.697V4.308c0-.63.692-1.01 1.233-.696l6.363 3.692a.802.802 0 0 1 0 1.393z"
          />
        </svg>
      </button>
      <button v-if="isPlay == true && isPause == false" @click="pause" class="pause btn">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-pause-fill"
          viewBox="0 0 16 16"
        >
          <path
            d="M5.5 3.5A1.5 1.5 0 0 1 7 5v6a1.5 1.5 0 0 1-3 0V5a1.5 1.5 0 0 1 1.5-1.5zm5 0A1.5 1.5 0 0 1 12 5v6a1.5 1.5 0 0 1-3 0V5a1.5 1.5 0 0 1 1.5-1.5z"
          />
        </svg>
      </button>
      <button v-if="isPause == true && isPlay == true" @click="stop" class="stop btn">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-stop-fill"
          viewBox="0 0 16 16"
        >
          <path
            d="M5 3.5h6A1.5 1.5 0 0 1 12.5 5v6a1.5 1.5 0 0 1-1.5 1.5H5A1.5 1.5 0 0 1 3.5 11V5A1.5 1.5 0 0 1 5 3.5z"
          />
        </svg>
      </button>
      <button v-if="isPlay == true && isPause == false" @click="mark" class="mark btn">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          viewBox="0 0 16 16"
        >
          <path
            d="M10.1328 4.77331L4.79948 2.46665V1.83331C4.79948 1.55998 4.57281 1.33331 4.29948 1.33331C4.02614 1.33331 3.79948 1.55998 3.79948 1.83331V14.1666C3.79948 14.44 4.02614 14.6666 4.29948 14.6666C4.57281 14.6666 4.79948 14.44 4.79948 14.1666V11.5266L10.2795 8.81998C10.2795 8.81998 10.2795 8.81998 10.2861 8.81998C11.3928 8.24665 11.9861 7.50665 11.9528 6.72665C11.9195 5.94665 11.2728 5.25331 10.1328 4.77331Z"
          />
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      time: 0,
      interval: null,
      isPlay: false,
      isPause: false,
      calcedTime: "00:00.00",
      trunCount: 1,
      trun: true,
      marked: []
    }
  },
  methods: {
    play(){
      this.interval = setInterval(() => {
        this.time = this.time + 1;
        this.calcedTime = this.calcTime(this.time);
      }, 10);
      this.isPlay = true;
      this.isPause = false;
      this.calcedTime = this.calcTime(this.time);
    },
    pause(){
      clearInterval(this.interval)
      this.isPause = true
    },
    stop(){
      this.time = 0,
      this.interval = null,
      this.isPlay = false,
      this.isPause = false,
      this.trunCount = 1,
      this.calcedTime = "00:00.00",
      this.marked = []
    },
    formatNumber(number){
      if(number <= 9){
        return (("0" + number).slice(-2));
      }else{
        return number;
      }
    },
    calcTime(time){
      return this.formatNumber(Math.floor(time/(99*60))) + ":" + this.formatNumber(Math.floor(time/99) > 59 ? Math.floor(time/99) - 60*Math.floor(time/(99*60)) : Math.floor(time/99)) + "." + this.formatNumber(Math.floor(time%100));
    },
    mark(markTime){
      let mark = {
        "id": this.marked.length,
        "time": this.time
      }
      this.marked.push(mark)
      console.log(this.marked)
    }
  }

};""
</script>

<style>
@import "https://library-battalsahin.netlify.app/reset.css";
body {
  width: 100vw;
  height: 100vh;
  background: #252525;
}

.page-hero {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: space-around;
  flex-direction: column;
}

.btn {
  width: 50px;
  height: 50px;
  padding: 10px;
  background: #404040;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  position: relative;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px, rgba(0, 0, 0, 0.3) 0px 18px 36px -18px;
  transition: all .25s ease 0s;
}

.btn:hover, .btn:focus{
  background: #555;
}

.btn:focus{
  box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px, rgba(0, 0, 0, 0.3) 0px 18px 36px -18px, 0 0 0 3px #0a5edd80;
}

.btn svg {
  width: 100%;
  height: 100%;
  color: #0a5edd;
}

.buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}
svg {
  vertical-align: middle;
}

.time {
  font-weight: 500;
  font-size: 40px;
  padding: 10px;
  color: #e5e5e5;
}

.marked {
  display: flex;
  align-items: center;
  flex-direction: column-reverse;
  gap: 5px;
  max-height: 275px;
  overflow: auto;
  padding: 5px;
  border-radius: 4px;
  transition: all .25s ease 0s;
  outline: none;
}

.marked:focus {
  background: #303030;
  box-shadow: rgb(10 94 221 / 50%) 0px 0px 0px 3px;
}

.marked li {
  max-width: 400px;
  width: 400px;
  height: 50px;
  padding: 15px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  background: #404040;
  border-radius: 5px;
  box-shadow: rgb(50 50 93 / 25%) 0px 30px 60px -12px, rgb(0 0 0 / 30%) 0px 18px 36px -18px;
}

@media screen and (max-width: 420px){  
  .marked{
    width: 95%;
  }
  
  .marked li{
    width: 100%;
  }
}


.turn{
  font-weight: 400;
  font-size: 16px;
  color: #cccccc;
}

.markTime{
  font-weight: 500;
  font-size: 16px;
  color: #e5e5e5;
}

::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: #404040;
  border-radius: 5px;
}

::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>