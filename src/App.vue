<template>
  <Navbar />
  <router-view/>
</template>

<script>
import * as bootstrap from 'bootstrap';
import Navbar from "@/components/Navbar";
export default({
 components: {Navbar},
  
})
</script>

<style lang="scss">
@import "app.scss";
</style>
