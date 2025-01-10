<script>
  import logoApp from './assets/images/logo.svg'
  import BillSplitterApp from './components/BillSplitter-app.vue';
  export default {
    components: {
      BillSplitterApp
    },
    data(){
      return{
        logo: logoApp
      }
    }
    
   
  };
</script>

<template>
  <div>
    <img :src="logo" alt="" class="logo">
    
  </div>
  <BillSplitterApp />
  
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #029553aa);
}
</style>
 