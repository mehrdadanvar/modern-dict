<template>
  <div class="dict">
    <h1>Dictionary Section</h1>
    <div class="request">
      <h4>Search for Meanings</h4>
      <form action="">
        <label for="">word: </label>
        <input class="input" type="text" placeholder="literate" name="text" v-model="text" />
        <button class="button" @click.prevent="call_dict">?</button>
      </form>
    </div>
    <div class="response">
      <h4>Definition</h4>
      <p v-if="one">{{ one.word }}</p>
      <div class="audio" v-if="one.phonetics">
        <audio controls>
          <source v-bind:src="this.one.phonetics[0].audio" type="audio/mp3" />
        </audio>
      </div>
      <div class="meanings" v-for="meaning in one_meanings" v-bind:key="meaning">
        <h4>meanings</h4>
        <hr />
        <div v-if="meaning">
          structure : <strong>{{ meaning.partOfSpeech }}</strong>
        </div>
        <div class="definitions">
          <h4>definitions</h4>
        </div>
        <div class="synonyms" v-if="meaning">
          <h4>synonyms</h4>
          <li v-if="meaning.synonyms.length > 0" v-for="item in meaning.synonyms" v-bind:key="item">
            {{ item }}
          </li>
        </div>
        <div class="antonyms" v-if="meaning">
          <h4>antonyms</h4>
          <li>{{ meaning.antonyms }}</li>
          <br />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
    name: "wordsView",
    components: {},
    data() {
      return {
        text: "",
        one: {},
        one_meanings: [],
        two: {},
      };
    },
    methods: {
      call_dict() {
        console.log("dict request");
        axios
          .get(`https://api.dictionaryapi.dev/api/v2/entries/en/${this.text}`)
          .then((response, error) => {
            const interest = response.data;
            if (error) console.log(error);
            const [first, second] = [...interest];
            this.one = "";
            this.one = first;
            this.one_meanings = first.meanings;
            //   console.log(first);
            this.second = second;
            let result = interest[0];
            //   console.log(result.meanings);
          })
          .catch((error) => console.log(error));
      },
    },
  };
</script>

<style scoped>
  .dict {
    background-color: rgb(240, 242, 246);
    margin: 0px;
  }
  .request {
    width: 400px;
    height: 200px;
    box-sizing: border-box;
    border: 2px solid grey;
    border-radius: 1rem;
    background-color: white;
  }
  .response {
    width: 400px;
    height: 700px;
    box-sizing: border-box;
    border: 3px solid grey;
    border-radius: 1rem;
    background-color: white;
    display: grid;
    grid-template-rows: 0.2fr 0.2fr 1fr 2fr 2fr;
  }
  .button {
    background-color: rgb(29, 175, 208);
    border-radius: 0.2rem;
  }
</style>
