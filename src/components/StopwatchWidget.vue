<template>
  <center>
    <div class="stopwatch">
      <div class="stopwatch-background"></div>
      <div class="display">
        {{ formatTime }}
      </div>
      <div class="controls">
        <button @click="start" :disabled="isRunning || elapsedTime > 0">Start</button>
        <button @click="split" :disabled="!isRunning">Split</button>
        <button @click="stop" :disabled="!isRunning">Stop</button>
        <button @click="reset" :disabled="elapsedTime === 0 || isRunning" :style="{ opacity: (elapsedTime === 0 || isRunning) ? '0.5' : '1' }">Reset</button>
      </div>
      
    </div>
    <div class="splits">
        <table>
          <thead>
            <tr>
              <th><b>SPLIT TIME</b></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(splitTime, index) in splitTimes" :key="index">
              <td><b>{{ splitTime }}</b></td>
            </tr>
          </tbody>
        </table>
      </div>
  </center>
</template>

<script>
export default {
  data() {
    return {
      isRunning: false,
      elapsedTime: 0,
      startTime: 0,
      timer: null,
      splitTimes: []
    };
  },
  computed: {
    formatTime() {
      const milliseconds = Math.floor(this.elapsedTime % 1000);
      const seconds = Math.floor((this.elapsedTime / 1000) % 60);
      const minutes = Math.floor((this.elapsedTime / (1000 * 60)) % 60);
      const hours = Math.floor((this.elapsedTime / (1000 * 60 * 60)) % 24);

      return (
        this.pad(hours, 2) +
        ":" +
        this.pad(minutes, 2) +
        ":" +
        this.pad(seconds, 2) +
        "." +
        this.pad(milliseconds, 3)
      );
    }
  },
  methods: {
    start() {
      if (!this.isRunning && this.elapsedTime === 0) {
        this.startTime = Date.now();
        this.timer = setInterval(() => {
          this.elapsedTime = Date.now() - this.startTime;
        }, 10);
        this.isRunning = true;
      }
    },
    split() {
      if (this.isRunning) {
        this.splitTimes.push(this.formatTime);
      }
    },
    stop() {
      if (this.isRunning) {
        clearInterval(this.timer);
        this.timer = null;
        this.isRunning = false;
      }
    },
    reset() {
      if (!this.isRunning) {
        this.elapsedTime = 0;
        this.startTime = 0;
        this.splitTimes = [];
      }
    },
    pad(value, length) {
      return value.toString().padStart(length, "0");
    }
  }
};
</script>

<style scoped>
.stopwatch {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: Arial, sans-serif;
  margin-top: -45px;
  height: 400px; /* Increased height to accommodate the splits table */
  width: 300px;
}

.stopwatch-background {
  position: absolute;
  top: 20;
  left: 10;
  width: 120%;
  height: 80%;
  background-color: #ffffff;
  border-radius: 50%;
  opacity: 0.9;
  z-index: -1;
}

.display {
  position: relative;
  font-size: 2rem;
  margin-bottom: 20px;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 10px;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  border-radius: 5px;
  background-color: #007bff;
  color: #ffffff;
  border: none;
  cursor: pointer;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.splits {
  margin-top: 0px;
  max-height: 200px;
  overflow-y: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  position: relative;
}

thead {
  position: sticky;
  top: 0;
  background-color: #ffffff;
}

th {
  z-index: 1;
  padding: 8px;
  text-align: left;
}

td {
  padding: 8px;
  text-align: left;
  color: #ffffff;
  font-style: bold;
}

td:first-child,
th:first-child {
  padding-left: 10px; /* Tambahkan padding kiri untuk kolom pertama */
}
</style>