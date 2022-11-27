<template>
  <div class="main">
    <div class="request">
      <label for="">Type Your Word</label>
      <input type="text" class="wordy" v-model="word" @keyup.enter="call_two_apis" placeholder="..." />
      <button @click="call_two_apis">Search</button>
    </div>
    <div class="response">
      <div v-for="meaning in meanings" :key="meaning">
        <div class="generals">
          <p>word : {{ meaning.word }}</p>
          <p>phonetic : {{ meaning.phonetic }}</p>
          <!-- <p>phonetics :{{ meaning.phonetics[0].audio }}</p> -->
          <div class="audio" v-if="meaning.phonetics">
            <audio controls>
              <source :src="meaning.phonetics[0].audio" type="audio/mp3" />
            </audio>
          </div>
        </div>
        <div class="meaning-container">
          <div v-for="x in meaning.meanings" :key="x" class="sample">
            <v-chip class="pos">{{ x.partOfSpeech }}</v-chip>
            <div v-for="k in x.definitions" :key="k">
              <li>{{ k.definition }}</li>
            </div>
            <div class="ant">
              <p v-for="ant in x.antonyms" :key="ant">
                {{ ant }}
              </p>
            </div>
            <div class="syn">
              <span v-for="syn in x.synonyms" :key="syn"> {{ syn }}, </span>
            </div>
          </div>
        </div>
      </div>
      <div class="thesarus">
        <h4>Thesarus Synonyms scraped</h4>
        <div class="th-words">
          <v-chip v-for="x in returned.data" :key="x" variant="elevated" class="mehrdad">{{ x }}</v-chip>
        </div>
      </div>
      <div class="corpos">
        <h4>Corpos Sentences Scraped</h4>
        <div class="th-corpos">
          <v-chip v-for="s in corpos.data" :key="s" variant="elevated" class="sentence">{{ s }}</v-chip>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  //request
  let word = ref("");
  let meanings = ref("");
  let returned = ref("");
  console.log(word.value);
  let corpos = ref("");
  async function call_two_apis() {
    if (word.value.length < 1) {
      console.log("not enough words");
    } else {
      // returned.value = "";
      await call_thesauri();
      // await call_dict();
    }
  }
  async function call_thesauri() {
    //part one inter call thesaurus
    returned.value = "";
    const pre_words = await fetch(`http://localhost:3000/api/thesarus?word=${word.value}`);
    let words = await pre_words.json();
    console.log(words, "comming from backend");
    returned.value = words;
    // part two external call dictionaryapi
    try {
      const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word.value}`);
      const info = await response.json();
      console.log(info);
      meanings.value = info;
      // word.value = "";
    } catch (error) {
      if (error) {
        console.log(error);
      }
    }
    //part three internal call corpus
    corpos.value = "";
    const corpos_response = await fetch(`http://localhost:3000/api/corpos?word=${word.value}`);
    console.log(corpos_response);
    const corpos_info = await corpos_response.json();
    console.log(corpos_info);
    corpos.value = corpos_info;
  }
  // async function call_dict() {
  //   try {
  //     const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word.value}`);
  //     const data = await response.json();
  //     console.log(data);
  //     meanings.value = data;
  //     word.value = "";
  //   } catch (error) {
  //     if (error) {
  //       console.log(error);
  //     }
  //   }
  // }
</script>

<style scoped>
  .sentence {
    background-color: rgb(220, 220, 220);
    color: black;
    font-size: 18px;
  }
  .mehrdad {
    background-color: salmon;
  }
  .main {
    display: grid;
    grid-template-rows: 1fr 10fr;
    color: grey;
  }
  .generals {
    display: flex;
    gap: 3rem;
  }
  .request {
    display: flex;
    flex-direction: row;
    align-items: center;
    max-width: 30rem;
    gap: 1rem;
    margin: 1rem 1rem;
    background-color: white;
  }
  .response {
    font-size: 20px;
    background-color: white;
    margin: 1rem 1rem;
  }
  .meaning-container {
    /* display: flex;
    flex-wrap: wrap;
    gap: 1rem; */
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
  }
  .sample {
    border: 1px solid grey;
    border-radius: 1rem;
    padding: 1rem 1rem;
    display: flex;
    flex-direction: column;
    max-width: 45rem;
  }
  .ant {
    color: brown;
  }
  .syn {
    color: green;
  }
  .pos {
    font-style: italic;
    font-size: 24px;
    border-radius: 1rem;
    margin: auto;
    padding: 1rem 1rem;
    max-height: 5rem;
  }
  .wordy {
    width: 15rem;
    height: 2rem;
    border: 1px solid salmon;
    border-radius: 0.5rem;
    padding-left: 1rem;
    color: rgb(0, 30, 255);
    font-size: 16px;
  }
  .wordy:focus {
    outline: none;
  }
  button {
    color: white;
    background-color: salmon;
    width: 7rem;
    height: 2rem;
    border: none;
    border-radius: 0.5rem;
  }
  button:hover {
    width: 8rem;
    transition: width 0.1s ease-in;
  }
  button:active {
    background-color: red;
  }
  .thesarus {
    margin: 1rem 1rem;
    border: 1px solid rgb(0, 255, 0);
    border-radius: 1rem;
    max-width: 40rem;
  }
  .th-words {
    /* display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    border-radius: 1rem;
    max-width: 40rem;
    color: white;
    background-color: rgb(161, 0, 0); */
  }
  .th-corpos {
    display: grid;
    grid-template-rows: 1fr 1fr 1fr;
    gap: 0.5rem;
  }
</style>
