<template>
  <footer>
    <p>Copright &copy; {{ new Date().getFullYear() }}</p>
  </footer>
</template>

<script>
export default {
  name: "Footer"
};
</script>

<style scoped>
a {
  color: #333;
}
footer {
  margin-top: 30px;
  text-align: center;
}
</style>
