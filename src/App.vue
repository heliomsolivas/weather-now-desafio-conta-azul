<template>
  <div id="app">
   <Header></Header>
   <main>
      <WeatherCard></WeatherCard>
   </main>
  </div>
</template>

<script>

import Header from './components/Header'
import WeatherCard from './components/WeatherCard'

export default {
  name: 'App',
  components: {
    Header,
    WeatherCard
  },
  data(){
    return{
    }
  }
}
</script>

<style lang="postcss">
#app{
  background-color: #f1f1f1;
}
@media (min-width: 768px){
  #app{
    height: 100%;
  }
}
</style>
