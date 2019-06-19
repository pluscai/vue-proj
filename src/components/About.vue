<template>
  <div class="about">
    <h1>Hello ! this is an about page</h1>
    <img src="../assets/4.jpg" style="max-width:90%" alt="">
     You can fork this demo in github: <a href="https://github.com/pluscai/vue-proj">https://github.com/pluscai/vue-proj</a>
  </div>
</template>

<script>
export default {
  name: "About",
  data() {
    return {
      their_name: this.$route.params.name
    };
  }
};
</script>