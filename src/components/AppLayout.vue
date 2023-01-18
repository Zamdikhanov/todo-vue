<template>
  <main class="main">
    <slot></slot>
  </main>
</template>

<script>
export default {
  name: 'AppLayout',
}
</script>

<style>
.main {
  max-width: 800px;
  margin: 32px auto;
  padding: 16px 32px;
  border-radius: 4px;
  box-shadow: 1px 1px 3px silver;
}
</style>