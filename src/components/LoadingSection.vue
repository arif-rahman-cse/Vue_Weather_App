<template>
  <section>
    <p class="text-center">
      <i class="fas fa-spinner fa-pulse"></i>
      {{ loadingmsg }}
    </p>
  </section>
</template>

<script>
export default {
  props: {
    loadingmsg: String,
  },
};
</script>

<style scoped></style>
