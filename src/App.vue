<script setup>
import { RouterLink, RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'

// console.log('hello');
//
// const apiKey = import.meta.env.VITE_OPENAI_API_KEY;
// const prompt = 'Once upon a time,';
// const maxTokens = 10;
// const apiUrl = 'https://api.openai.com/v1/engines/ada/completions';
//
// const requestOptions = {
//   method: 'POST',
//   headers: {
//     'Content-Type': 'application/json',
//     'Authorization': `Bearer ${apiKey}`
//   },
//   body: JSON.stringify({
//     prompt,
//     max_tokens: maxTokens
//   })
// };
//
// fetch(apiUrl, requestOptions)
//     .then(response => response.json())
//     .then(data => console.log(data))
//     .catch(error => console.error(error));
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/home">
          Page 1
        </RouterLink>

        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
