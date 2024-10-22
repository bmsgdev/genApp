
<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import { GoogleGenerativeAI } from "@google/generative-ai";
import { ref} from "vue";
import { marked } from 'marked';

// Fetch your API_KEY
const API_KEY = "AIzaSyDMd_M7ZrjIL6TqiJwPh8SFYEm6YEid9RU";
// const genAI = new GoogleGenerativeAI(process.env.API_KEY);
const genAI = new GoogleGenerativeAI(API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });
let prompt = ref('');

let result = ref('');
async function getResult() {
   model.generateContent(prompt.value).then(
    (r) => {
      result.value=marked(r.response.candidates[0].content.parts[0].text) ;
      console.log(result.value);
      
    },
    (error) => {
      console.log(error);
    }
   );

  
}
// console.log(result.response.text());
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <input type="text" v-model="prompt">
        <button @click="getResult()">soumettre</button>
        <p v-html="result"></p>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
@tailwind base;
@tailwind components;
@tailwind utilities;
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
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
