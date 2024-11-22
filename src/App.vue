<script setup>
import { ref } from "vue"

const data = ref([])

// npm run dev
async function getData(word) {
  const url = `https://api.dictionaryapi.dev/api/v2/entries/en/${word}`;
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    console.log(json);
    data.value = json
  } catch (error) {
    console.error(error.message);
  }
}
</script>

<template>
  <div class="wrapper">
    <header>
      <img src="./assets/logo.svg" />
    </header>


    <div class="buscador_div">
      <input type="text" id="buscador" @input="getData($event.target.value)" />
      <div class="otro_div">
        <img src="./assets/icon-search.svg" class="lupa" />
      </div>
    </div>

    <section id="titulo">
      <div>
        <h1>keyboard</h1>
        <h4>/"ki:bↄ:d/</h4>
      </div>
      <img src="./assets/icon-play.svg" />
    </section>

    <div class="noun">
      <h5>noun</h5>
      <div class="line"></div>
    </div>

    <div class="meaning">
      <h4 style="color: var(--clr-primary-400);">Meaning</h4>
      <ul>
        <li>(etc.) A set of keys used to operate a typewriter, computer etc.</li>
        <li>A component of many instruments including the piano, organ, and hardsichord consisting of usually black and
          white keys that cause different tones to be produced when struck.</li>
        <li>A device with keys of a musical keyboard, used to control electronic sound-producing devices which may be
          built into or separate from the keyboard device.</li>
      </ul>
    </div>

    <div class="noun">
      <h4 style="color: var(--clr-primary-400);">Synonyms</h4>
      <h4 style="color: var(--clr-secondary-100)">electronic keyboard</h4>
    </div>

    <div class="noun">
      <h5>verb</h5>
      <div class="line"></div>
    </div>

    <div class="meaning">
      <h4 style="color: var(--clr-primary-400);">Meaning</h4>
      <ul>
        <li>To type on a computer keyboard.</li>
        <div id="transparente" style="color: var(--clr-primary-400);">
          <li>"keyboarding is the part of this job I hate the most."</li>
        </div>
      </ul>
    </div>
    <hr style="color: var(--clr-primary-400);;">

    <footer>
      <p style="color: var(--clr-primary-400);">Source</p>
      <a style="color: black;" href="https://en.wiktionary.org/wiki/keyboard">https://en.wiktionary.org/wiki/keyboard</a>
    </footer>

    <div>
      <pre>
        {{ data }}
      </pre>
    </div>

  </div>
</template>


<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.wrapper {
  max-width: 500px;
  margin: 0 auto;

  display: flex;
  flex-direction: column;
  gap: 30px;
}


header {
  display: flex;
  align-items: center;
  /* background-color: red; */
}

form {
  margin-left: auto;
}

#titulo {
  margin-left: auto;
  text-align: center;
  /* background-color: blue; */

  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.buscador_div {
  position: relative;

}

#buscador {
  /* background-color: aqua; */
  width: 100%;
  padding: 5px;
  border-radius: 20px;

}

.lupa {

  /* background-color: brown; */

  /* right: 0; */
}


.otro_div {
  /* background-color: green; */
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;

  display: flex;
  align-items: center;
}



.noun {
  /* background-color: aquamarine; */

  display: flex;
  align-items: center;
  gap: 10px;
  /* background-color: beige; */



}

.line {
  height: 1px;
  width: 100%;
  background-color: var(--clr-primary-400);
}

.meaning {
  /* background-color: blueviolet; */
  list-style-position: inside;
}

#transparente {
  list-style-type: none;
  padding-left: 20px;

}

footer {
  display: flex;
  align-items: center;
  gap: 10px;
}
</style>
