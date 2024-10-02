<template>
  <div id="app">
    <PullsTicker :chances="chances" :characters="characters"/>
  </div>
</template>

<script>
import PullsTicker from './components/PullsTicker.vue';

export default {
  name: 'app',
  data() {
    return {
      characters: [],
      chances: []
    }
  }, 
  components: {
    PullsTicker
  },
  mounted(){
    fetch('characters.json').then(resp=>resp.json()).then(json=>{
      this.characters=json;
      console.log(json);
    }),
    fetch('chances.json').then(resp=>resp.json()).then(json=>{
      this.chances=json;
      console.log(json);
    })
  } 
}
</script>

<style>

</style>
