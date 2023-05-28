<template>
<Banner />

  <div class="main-container">
    <h1>Monte o seu Pedido:</h1>
    <Pastel/>    
  </div>
</template>

<script>
import Banner from "../components/Banner.vue"
import Pastel from "../components/Pastel.vue"

export default{
  name: "Home",
  components:{
    Banner,
    Pastel
  }
}

</script>
