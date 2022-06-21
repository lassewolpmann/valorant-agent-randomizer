<template>
  <div class="wheel">
    <div class="current-agent-indicator">
      <span class="arrow upper"></span>
      <span class="arrow lower"></span>
    </div>
    <TransitionGroup name="agents">
      <div v-for="agent in agents" :key="agent" class="agent">
        <img :src="agent['bustPortrait']" :alt="agent['displayName']">
        <p>{{ agent.displayName.toUpperCase() }}</p>
      </div>
    </TransitionGroup>
  </div>
  <button v-on:click="pick" class="roll-button">ROLL AGENT</button>
</template>

<script>
export default {
  name: "AgentWheel",
  data() {
    return {
      agents: []
    };
  },
  methods: {
    shuffle() {
      let agents = [...this.agents];
      let currentIndex = agents.length,  randomIndex;

      // While there remain elements to shuffle.
      while (currentIndex !== 0) {

        // Pick a remaining element.
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        // And swap it with the current element.
        [agents[currentIndex], agents[randomIndex]] = [
          agents[randomIndex], agents[currentIndex]];
      }

      this.agents = agents;
    },
    async pick() {
      let rollCount = Math.floor(Math.random() * (50 - 5) + 5);
      for (let i = 0; i < rollCount; i++) {
        this.agents.push(this.agents.shift());
        await new Promise(r => setTimeout(r, 50));
      }
    }
  },
  mounted() {
    fetch("https://valorant-api.com/v1/agents")
        .then(response => response.json())
        .then(data => (this.agents = data.data.filter(agent => agent['isPlayableCharacter'])))
        .then(() => this.shuffle(this.agents))
  },
}
</script>

<style scoped>
  .agents-move {
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

  .current-agent-indicator {
    position: absolute;
    width: 250px;
    height: 322px;
    border: 3px solid #FF1145;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1;
  }

  .current-agent-indicator > .arrow {
    position: absolute;
    width: 20px;
    height: 10px;
    background: rgb(220, 220, 220);
    border-radius: 5px;
  }

  .current-agent-indicator > .arrow.upper {
    clip-path: polygon(0 0, 100% 0, 50% 100%);
    top: -5px;
  }

  .current-agent-indicator > .arrow.lower {
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

  .agent {
    margin: 5px;
    background: black;
    border-radius: 5px;
  }

  .agent > img {
    width: 250px;
    height: auto;
    aspect-ratio: 1/1;
    padding: 10px 0;
  }

  .agent > p {
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