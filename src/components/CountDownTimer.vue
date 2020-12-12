<template>
<div>
  <div v-if="!isTicking">
    <label for="minute">minute</label>

    <input
      type="number"
      id="number-field"
      v-model="minute"
      placeholder="0"
      maxlength="2"
      min="0"
      max="99"
    >

    <label for="minute">second</label>

    <input
      type="number"
      id="number-field"
      v-model="second"
      placeholder="0"
      maxlength="2"
      min="0"
      max="60"
    >
  </div>

  <button class="action-button" @click="startTimer" :disabled="isTicking">Start</button>

  <button class="action-button" @click="stopTimer" v-if="isTicking">Stop</button>

  <br>
  <div id="bar-container">
    <progress :value="progress" :max="totalSecond" />

    <p>{{ timeLeft }}</p>
  </div>
</div>
</template>

<script>
export default {
  name: 'CountDownTimer',
  computed: {
    totalSecond() {
      return parseInt(this.minute, 10) * 60 + parseInt(this.second, 10);
    },
    timeLeft() {
      if (this.progress >= 60) {
        return `${parseInt(this.progress / 60, 10)} min
        ${parseInt(this.progress, 10) % 60} sec`;
      }

      return `${parseInt(this.progress, 10) % 60} sec`;
    },
  },
  data() {
    return {
      progress: 0,
      minute: 0,
      second: 0,
      timer: null,
      isTicking: false,
    };
  },
  methods: {
    startTimer() {
      if (this.totalSecond === 0) return;

      this.isTicking = true;
      this.progress = this.totalSecond;
      this.timer = setInterval(() => {
        if (this.progress === 0) {
          this.isTicking = false;
          clearInterval(this.timer);
        } else {
          this.progress -= 1;
        }
      }, 1000);
    },
    stopTimer() {
      this.isTicking = false;
      clearInterval(this.timer);
    },
  },
};

</script>

<style scoped>
#number-field {
  border: 0px;
  border-bottom: 1px solid black;
  margin: 0 8px;
  font-size: 2em;
  width: 2em;
}

#bar-container {
  margin: 5em 0;
}

.action-button {
  margin: 1em;
}

</style>
