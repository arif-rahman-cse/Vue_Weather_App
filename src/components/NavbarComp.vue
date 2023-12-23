<template>
  <!-- Image and text -->
  <div class="my-5 container-fluid" id="nav_bar">
    <img
      :src="imageUrl"
      width="64"
      height="64"
      class="align-center"
      alt=""
    />
    <span >{{ title }}</span>
  </div>
</template>

<script>
import logo from "@/assets/logo.svg"
export default {
  props: {
    title: String,
  },
  data() {
    return {
      imageUrl: logo,
    };
  },
};
</script>

<style scoped>
#nav_bar {
  /* Liner gradient color for nav bar */
   background-color: #8ec5fc;
   background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
  /* background: white; */
  display: flex;
  justify-content: center;
  text-align: center;
  align-items: center;
  text-align: center; 
  padding: 2px;
  border: 1px solid #fafafa;
  border-radius: 3rem;
  /*  0, 0, 0 is black (RGB), and 0.35 is the alpha (transparency) value. 
  0px: horizontal offset of the shadow.
  5px: vertical offset of the shadow
  15px: blur radius of the shadow. 
  */
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px; 
}
span  {
  margin-left: 10px;
  font-size: large;
  font-weight: bold;
}
</style>
