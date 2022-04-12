<template>
<Banner />
  <section class="main-container">
    <h1>Monte o seu Burger</h1>
    <BurgerForm />
  </section>
</template>

<script>
import Banner from "@/components/Banner.vue";
import BurgerForm from "@/components/BurgerForm.vue";

export default {
  name: 'HomeView',
  components: {
    Banner,
    BurgerForm,
  }
}
</script>
