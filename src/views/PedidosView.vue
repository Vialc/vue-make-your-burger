<template>
  <section class="main-container">
    <h1>Gerenciar Pedidos:</h1>
    <Dashboard />
  </section>
  
</template>

<script>
import Dashboard from "@/components/Dashboard.vue"


export default {
  name: 'Pedidos',
  components: {
    Dashboard
  }
}
  
</script>