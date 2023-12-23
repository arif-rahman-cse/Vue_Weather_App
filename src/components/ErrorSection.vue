<template>
    <section>
      <p class="text-center">{{ errormsg }}</p>
    </section>
  </template>
  
  <script>
  export default {
    props: {
      errormsg: String,
    },
  };
  </script>
  
  <style scoped></style>
  