<template>
  <div class="wheel">
    <div class="current-indicator">
      <span class="arrow upper"></span>
      <span class="arrow lower"></span>
    </div>
    <TransitionGroup name="wheel">
      <div v-for="data in wheelData" :key="data" class="data">
        <img :src="data['displayIcon']" :alt="data['displayName']" :style="{ transform: 'translateX(' + imgOffset + 'px)'}">
        <p :style="{ transform: 'translateX(' + imgOffset + 'px)'}">{{ data.displayName.toUpperCase() }}</p>
      </div>
    </TransitionGroup>
  </div>
  <button v-on:click="pick" class="roll-button">ROLL {{ category }}</button>
</template>

<script>
export default {
  name: "BasicWheel",
  props: {
    category: String,
    apiURL: String
  },
  data() {
    return {
      wheelData: [],
      imgOffset: null
    };
  },
  methods: {
    shuffle() {
      let data = [...this.wheelData];
      let currentIndex = data.length,  randomIndex;

      // While there remain elements to shuffle.
      while (currentIndex !== 0) {

        // Pick a remaining element.
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        // And swap it with the current element.
        [data[currentIndex], data[randomIndex]] = [
          data[randomIndex], data[currentIndex]];
      }

      this.wheelData = data;
    },
    async pick() {
      let rollCount = Math.floor(Math.random() * (50 - 5) + 5);
      for (let i = 0; i < rollCount; i++) {
        this.wheelData.push(this.wheelData.shift());
        await new Promise(r => setTimeout(r, 50));
      }
    }
  },
  mounted() {
    fetch(this.apiURL)
        .then(response => response.json())
        .then(data => (this.wheelData = data.data))
        .then(() => this.shuffle(this.wheelData))
        .then(() => this.wheelData.length % 2 === 0 ? this.imgOffset = 125 : this.imgOffset = 0)
  },
}
</script>

<style scoped>
.wheel-move {
  transition: all 100ms;
}

.wheel {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  width: 100%;
  position: relative;
  padding: 30px 0;
}

.current-indicator {
  position: absolute;
  width: 250px;
  height: 90%;
  border: 3px solid #FF1145;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
}

.current-indicator > .arrow {
  position: absolute;
  width: 20px;
  height: 10px;
  background: rgb(220, 220, 220);
  border-radius: 5px;
}

.current-indicator > .arrow.upper {
  clip-path: polygon(0 0, 100% 0, 50% 100%);
  top: -5px;
}

.current-indicator > .arrow.lower {
  clip-path: polygon(50% 0, 100% 100%, 0 100%);
  bottom: -5px;
}

.wheel::after {
  position: absolute;
  content: '';
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, rgba(0, 0, 0, 0.8) 10%, rgba(0, 0, 0, 0.1) 50%, rgba(0, 0, 0, 0.8) 90%);
}

.data {
  margin: 5px;
  border-radius: 5px;
}

.data > img {
  width: 250px;
  height: auto;
  padding: 10px 0;
}

.data p {
  color: white;
  font-weight: bold;
  font-size: 1.5em;
  margin: 10px 0;
}

.roll-button {
  all: unset;
  margin-top: 20px;
  background: #FF1145;
  color: white;
  font-weight: bold;
  font-size: 1.5em;
  padding: 20px 75px;
  cursor: pointer;
  border-radius: 3px;
}

.roll-button:hover {
  background: #111111;
}
</style>