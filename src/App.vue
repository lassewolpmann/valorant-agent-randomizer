<template>
  <div class="category-buttons">
    <button @click="category = 'agents'">Agents</button>
    <button @click="category = 'guns'">Guns</button>
  </div>
  <AgentWheel v-if="category === 'agents'" />
  <GunWheel v-if="category === 'guns'"  />
  <PageFooter />
</template>

<script>
import AgentWheel from "@/components/Wheel/AgentWheel";
import GunWheel from "@/components/Wheel/GunWheel";
import PageFooter from "@/components/PageFooter";

export default {
  name: 'App',
  data() {
    return {
      category: 'agents'
    }
  },
  components: {
    PageFooter,
    GunWheel,
    AgentWheel
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

.category-buttons {
  position: absolute;
  top: 50px;
}

.category-buttons > button {
  all: unset;
  margin: 10px 20px;
  background: #FF1145;
  color: white;
  font-weight: bold;
  font-size: 1.5em;
  padding: 10px 25px;
  cursor: pointer;
  border-radius: 3px;
}

.category-buttons > button:hover {
  background: #111111;
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
