<template>
  <BasicWheel category="AGENTS" apiURL="https://valorant-api.com/v1/agents?isPlayableCharacter=true"/>
</template>

<script>
import BasicWheel from "@/components/Wheel/BasicWheel";

export default {
  name: "GunWheel",
  components: {BasicWheel}
}
</script>

<style scoped>
</style>