<template>
  <div class="wheel">
    <div class="current-indicator">
      <span class="arrow upper"></span>
      <span class="arrow lower"></span>
    </div>
    <TransitionGroup name="wheel">
      <div v-for="data in wheelData" :key="data" class="data" :class="{ offset: imgOffset} ">
        <img v-if="category === 'guns'" :src="data['displayIcon']" :alt="data['displayName']">
        <img v-if="category === 'agents'" :src="data['fullPortraitV2']" :alt="data['displayName']" :style="getBackground(data)">
        <p>{{ data.displayName.toUpperCase() }}</p>
      </div>
    </TransitionGroup>
  </div>
  <button v-on:click="pick" class="roll-button">ROLL {{ category.toUpperCase() }}</button>
</template>

<script>
export default {
  name: "BasicWheel",
  props: {
    data: Array,
    category: String
  },
  methods: {
    shuffle() {
      let currentIndex = this.wheelData.length,  randomIndex;
      while (currentIndex !== 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        [this.wheelData[currentIndex], this.wheelData[randomIndex]] = [
          this.wheelData[randomIndex], this.wheelData[currentIndex]];
      }
    },
    async pick() {
      let rollCount = Math.floor(Math.random() * (50 - 5) + 5);
      for (let i = 0; i < rollCount; i++) {
        this.wheelData.push(this.wheelData.shift());
        await new Promise(r => setTimeout(r, 50));
      }
    },
    getBackground(data) {
      let gradientColors = {...data['backgroundGradientColors']};
      let gradientOne = '#' + gradientColors[0];
      let gradientTwo = '#' + gradientColors[1];
      let gradient = gradientOne + ', ' + gradientTwo;
      let backgroundImage = data['background'];

      return 'background-image: url(' + backgroundImage + '), linear-gradient(90deg, ' + gradient + ')'
    }
  },
  data() {
    return {
      wheelData: Array,
      imgOffset: false
    }
  },
  mounted() {
    this.wheelData = this.data;
    this.shuffle(this.wheelData);
    this.wheelData.length % 2 === 0 ? this.imgOffset = true : this.imgOffset = false;
  }
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
  width: 290px;
  height: 95%;
  border: 2px solid #FF1145;
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
  background: #FF1145;
}

.current-indicator > .arrow.upper {
  clip-path: polygon(0 0, 100% 0, 50% 100%);
  top: -1px;
}

.current-indicator > .arrow.lower {
  clip-path: polygon(50% 0, 100% 100%, 0 100%);
  bottom: -1px;
}

.wheel::after {
  position: absolute;
  content: '';
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0) 50%, rgba(0, 0, 0, 0.5) 100%);
}

.data {
  min-width: 250px;
  height: 300px;
  margin: 0 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.data.offset {
  transform: translateX(145px);
}

.data > img {
  width: 80%;
  height: auto;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 0 0 3px 3px rgba(0, 0, 0, 0.3);
  background-position: center;
  background-size: cover;
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
  background: linear-gradient(90deg, #0F1923 50%, #FF1145 50%) right bottom;
  background-size: 201% 100%;
  color: white;
  font-weight: bold;
  font-size: 1.5em;
  padding: 20px 75px;
  cursor: pointer;
  transition:all 0.3s ease;
}

.roll-button:hover {
  background-position: left bottom;
}
</style>