<template>
  <div class="dict">
    <h1>Dictionary Section</h1>
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
