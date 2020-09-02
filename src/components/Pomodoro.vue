<template>
  <div id="pomodoro">
    <h1 class="top">Vue Pomodoro Clock</h1>
    <h1 class="top">Cycles Done: {{count}}</h1>

    <div v-if="view=='main'" class="wrapper">
      <div class="circle-3"></div>
      <div class="circle-2"></div>
      <div class="circle-1">
        <h3>{{text}}</h3>
        <h3 v-if="!breaks">{{minutes}}:{{seconds}}</h3>
      </div>

      <div class="btns">
        <button @click="resetTimer">Restart</button>
        <button v-if="!timer" @click="startTimer">{{state}}</button>
        <button v-else @click="stopTimer">{{state}}</button>

        <button @click="view='change'">Adjust Time</button>
        <button @click="view='info'">Info</button>
      </div>
    </div>
    <!-- <div class="btns">
      <button @click="resetTimer">Restart</button>
      <button v-if="!timer" @click="startTimer">{{state}}</button>
      <button v-else @click="stopTimer">{{state}}</button>

      <button @click="view='change'">Adjust Time</button>
      <button @click="view='info'">Info</button>
    </div>-->
    <!-- part 2:  info  -->
    <div class="info" v-if="view=='info'">
      <h1>What Is The Pomodoro Technique?</h1>
      <p>The pomodoro technique is a time management method that uses a timer to break down work into intervals separated by short breaks.</p>
      <p>Usually by 25 minutes, but I made it 5 seconds to test the functionality of the app.</p>

      <span>
        <button @click="view = 'main'">Go Back</button>
      </span>
    </div>

    <!-- part 2:  adjust time  -->
    <div class="change" v-if="view=='change'">
      <h1>Adjust The Pomodoro Timer</h1>
      <span>
        <input
          type="number"
          v-model="newTime"
          placeholder="how many minutes"
          @keypress.enter="done"
        />
      </span>
      It's completely up to you however long you want one cycle to be!
      <span>
        <button @click="done">Done</button>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Pomodoro",
  data() {
    return {
      count: 0,
      text: "Work",
      state: "Start",
      totalTime: 5,
      timer: null,
      restartTime: 5,
      view: "main",
      newTime: null,
      breaks: false,
    };
  },
  methods: {
    resetTimer() {
      this.totalTime = this.restartTime;
      clearInterval(this.timer);
      this.timer = null;
    },
    startTimer() {
      // it should call the .countdown method every second
      this.timer = setInterval(() => this.countdown(), 1000);
      this.state = "Pause";
      this.text = "Work";
      this.breaks = false;
    },
    stopTimer() {
      this.state = "Continue";
      clearInterval(this.timer);
      this.timer = null;
    },
    countdown() {
      if (this.totalTime >= 1) {
        this.totalTime--;
      } else {
        this.totalTime = 0;
        this.count++;
        this.text = "Take a short break!";
        this.resetTimer();
        this.breaks = true;
        this.state = "End break";
      }
    },
    done() {
      this.view = "main";
      this.totalTime = this.newTime * 60;
      this.restartTime = this.newTime * 60;
    },
    padTime: function (time) {
      return (time < 10 ? "0" : "") + time;
    },
  },
  computed: {
    minutes: function () {
      let minutes = Math.floor(this.totalTime / 60);
      return this.padTime(minutes);
    },
    seconds: function () {
      let seconds = this.totalTime - Math.floor(this.minutes * 60);
      return this.padTime(seconds);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.top {
  text-align: center;
  color: white;
  font-size: 30px;
  margin: 10px;
}

.wrapper {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  margin-top: -100px;
}

.circle-2 {
  border: 6px solid white;
  border-radius: 50%;
  padding: 120px;
  position: absolute;
}

.circle-3 {
  border: 2px solid lightgray;
  border-radius: 50%;
  padding: 220px;
  position: absolute;
}

h3 {
  text-align: center;
  color: white;
}

.btns {
  display: flex;
  justify-content: space-around;
  position: absolute;
  width: 700px;
  bottom: 10px;
}

button {
  border-radius: 50%;
  padding: 10px;
  justify-content: center;
  align-items: center;
  background: #42b883;
  color: white;
  width: 110px;
  height: 100px;
  outline: none;
  font-size: 15px;
  border: 2px solid lightgray;
}

button:hover {
  border: 2px solid white;
}

.info {
  text-align: center;
  color: white;
  margin-top: 150px;
  display: flex;
  flex-direction: column;
  height: 50vh;
  justify-content: space-evenly;
}

.change {
  text-align: center;
  color: white;
  margin-top: 150px;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  height: 50vh;
}

.change h1 {
  font-size: 40px;
}

.change p {
  font-size: 20px;
}

input {
  width: 500px;
  padding: 20px;
  font-size: 20px;
  text-align: center;
}
</style>
