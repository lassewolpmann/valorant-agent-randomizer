<template>
  <div class="category-buttons">
    <button @click="category = 'agents'">AGENTS</button>
    <button @click="category = 'guns'">GUNS</button>
  </div>
  <BasicWheel v-if="category === 'agents'" :data="agentData" :category="category" />
  <BasicWheel v-if="category === 'guns'"  :data="gunData" :category="category" />
  <PageFooter />
</template>

<script>
import PageFooter from "@/components/PageFooter";
import BasicWheel from "@/components/BasicWheel";

export default {
  name: 'App',
  components: {
    BasicWheel,
    PageFooter
  },
  data() {
    return {
      category: null,
      agentData: Array,
      gunData: Array
    }
  },
  async mounted() {
    let agentResponse = await fetch("https://valorant-api.com/v1/agents?isPlayableCharacter=true");
    let agentData = await agentResponse.json();
    this.agentData = agentData.data;

    let gunResponse = await fetch("https://valorant-api.com/v1/weapons");
    let gunData = await gunResponse.json();
    this.gunData = gunData.data;

    this.category = 'agents';
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
}

.category-buttons {
  margin-bottom: 20px;
  display: flex;
  flex-direction: row;
}

.category-buttons > button {
  all: unset;
  flex: 1;
  margin: 0 20px;
  background: linear-gradient(90deg, #0F1923 50%, #FF1145 50%) right bottom;
  background-size: 200% 100%;
  color: white;
  font-weight: bold;
  font-size: 1.5em;
  padding: 10px 25px;
  cursor: pointer;
  transition:all 0.3s ease;
}

.category-buttons > button:hover {
  background-position: left bottom;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  min-height: 100vh;
  height: auto;
  width: 100vw;

  background: rgb(255,68,101);
  background: linear-gradient(90deg, rgba(255, 68, 101, 1) 0%, rgba(50, 50, 50, 1) 100%);
}
</style>
