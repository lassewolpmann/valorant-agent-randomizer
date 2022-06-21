<template>
  <AgentWheel />
  <PageFooter />
</template>

<script>
import AgentWheel from "@/components/AgentWheel";
import PageFooter from "@/components/PageFooter";

export default {
  name: 'App',
  components: {
    PageFooter,
    AgentWheel
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
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
