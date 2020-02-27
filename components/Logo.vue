<template>
  <div class="VueToNuxtLogo">
    <FetchApi 
      method="GET"
      endpoint="https://swapi.co/api/people/1"
      v-slot="{ data }"
    >
      <pre>
        {{ data }}
      </pre>
    </FetchApi>
  </div>
</template>

<script>
  import FetchApi from '@/components/renderless/FetchApi'

  export default {
    name: 'Logo',
    components: {
      FetchApi
    }
  }
</script>

<style>
.VueToNuxtLogo {
  display: inline-block;
  animation: turn 2s linear forwards 1s;
  transform: rotateX(180deg);
  position: relative;
  overflow: hidden;
  height: 180px;
  width: 245px;
}

@keyframes turn {
  100% {
    transform: rotateX(0deg);
  }
}
</style>
