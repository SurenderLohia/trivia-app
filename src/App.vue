<template>
  <div id="app" class="App bg-gradient">
      <div class="container expand-height">
        <div class="section expand-height">
          <div class="columns is-centered expand-height">
            <div class="column is-8 flex-row is-center">
              <Trivia />
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import Trivia from './components/Trivia.vue'

export default {
  name: 'App',
  components: {
    Trivia
  }
}
</script>

<style src="bulma/css/bulma.css"></style>
<style src="./assets/css/main.css"></style>