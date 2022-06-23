<template>
  <footer>
    <a href="https://github.com/lassewolpmann/valorant-agent-randomizer" target='_blank' rel="noopener noreferrer">GitHub</a>
  </footer>
</template>

<script>
export default {
  name: "PageFooter"
}
</script>

<style scoped>
  footer {
    margin-top: 30px;
  }

  footer > a {
    all: unset;
    cursor: pointer;
    font-weight: bold;
    color: white;
    background: rgba(255, 255, 255, 0.3);
    padding: 5px 10px;
  }
</style>