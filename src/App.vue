<template>
  <div id="app">
    <h1 class="first-content animate__animated animate__fadeIn">URL Shortener</h1>
    <h3 class="creator-content animate__animated animate__fadeIn">Developed by <a href="https://rmzn.netlify.app" target="_blank">Ramazan Azimli</a></h3>
    <form @submit.prevent="shortenUrl">
      <input type="url" class="animate__animated animate__fadeIn" v-model="url" placeholder="Enter URL" required />
      <button type="submit" class="animate__animated animate__fadeIn"><span>Shorten</span></button>
    </form>
    <div class="action-container animate__animated animate__fadeIn">
    <p v-if="shortUrl" class="action-template animate__animated animate__fadeIn">Shorted URL : <a :href="shortUrl" target="_blank" class="action-template-router animate__animated animate__fadeIn">{{ shortUrl }}</a></p>
    <p v-if="error">{{ error }}</p>
  </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
  data() {
    return {
      url: '',
      shortUrl: '',
      error: '',
    };
  },
  methods: {
    async shortenUrl() {
      try {
        const response = await axios.get(`https://api.shrtco.de/v2/shorten?url=${this.url}`);
        if (response.data.ok) {
          this.shortUrl = response.data.result.full_short_link;
          this.error = '';
        } else {
          this.error = 'Failed to shorten the URL. Please try again.';
        }
      } catch (error) {
        this.error = 'An error occurred. Please try again.';
      }
    },
  },
});
</script>

<style>
#app {
  text-align: center;
  padding: 30px;
}

input {
  padding: 15px 16px;
  font-size: 17px;
  width: 300px;
  outline: none;
  color: #ffff;
  user-select: none;
  font-weight: bold;
  background: none;
  border-radius: 15px;
  font-family: 'Chakra Petch', sans-serif;
  border: 2px solid #0177ff;
}
.action-template-router {
  color: #0177ff;
  font-size: 20px;
  font-weight: bold;
}
button {
 outline: none;
 cursor: pointer;
 border: none;
 padding: 16px 22px;
 margin-left: 7px;
 font-family: inherit;
 font-size: inherit;
 position: relative;
 display: inline-block;
 letter-spacing: 0.05rem;
 font-weight: 700;
 font-size: 17px;
 border-radius: 15px;
 overflow: hidden;
 background: #ffff;
 color: ghostwhite;
}

button span {
 position: relative;
 z-index: 10;
 transition: color 0.4s;
}

button:hover span {
 color: black;
}

button::before,
button::after {
 position: absolute;
 top: 0;
 left: 0;
 width: 100%;
 height: 100%;
 z-index: 0;
}
.action-container {
 filter: drop-shadow(0 0 0.75rem #0177ff);

}
button::before {
 content: "";
 background: #0177ff;
 width: 120%;
 left: -10%;
 transform: skew(30deg);
 transition: transform 0.4s cubic-bezier(0.3, 1, 0.8, 1);
}

button:hover::before {
 transform: translate3d(100%, 0, 0);
 filter: drop-shadow(0 0 0.75rem #0177ff);
}
.creator-content {
  margin: 7px 0;
}
.creator-content a {
  color: #ffff;
}
.first-content {
  margin: 7px 0;
}
.action-template {
  font-weight: bold;
  font-size: 20px;
}
@media only screen and (max-width: 700px) {
  input {
    width: 54%;
  }
  #app {
  text-align: center;
  padding: 7px;
  margin-top: 10vh;
}
button {
  widows: 30%;
  cursor: none;
}
.creator-content {
  margin: 14px 0;
}
}
</style>
