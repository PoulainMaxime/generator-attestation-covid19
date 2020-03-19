<template>
  <div class="home">
    <Form msg="Welcome to My Vue.js App" />
  </div>
</template>

<script>
import Form from "@/components/Form.vue";

export default {
  name: "Home",
  components: {
    Form
  }
};
</script>
