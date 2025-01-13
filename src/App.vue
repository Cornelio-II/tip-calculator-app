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
    <img :src="logo" alt="billSplitterlogo.svg" class="logo">
    
    <BillSplitterApp />

  <div class="attribution">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>.
    Coded by <a href="#">Cornelio</a>.
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em hsl(191, 94%, 14%));
}
.attribution {
  margin: 10px auto;
  color: var(--very-dark-cyan)
}</style>
 