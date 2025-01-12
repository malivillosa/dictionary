<script setup>
import { computed, onMounted, ref } from "vue"

const data = ref([])
const isPlaying = ref(false)
const word = ref("")

const audioUrl = computed(() => {
  return data.value?.[0]?.phonetics.find(item => item.audio)?.audio
})

async function getData(word) {
  const url = `https://api.dictionaryapi.dev/api/v2/entries/en/${word}`;
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }
    const json = await response.json();
    data.value = json
  } catch (error) {
    console.error(error.message);
    data.value = []
  }
}

function reproducirAudio() {
  if (!audioUrl.value) return
  if (isPlaying.value) {
    return
  }

  isPlaying.value = true

  const audio = new Audio(audioUrl.value);
  audio.play();

  audio.addEventListener('ended', () => {
    isPlaying.value = false
  })
}

function getRandomWord() {
  const words = [
    "polyglot",
    "absquatulate",
    "word"
  ]

  return words[Math.floor((Math.random() * words.length))]
}

onMounted(() => {
  const randomWord = getRandomWord()
  word.value = randomWord
  getData(randomWord)
})
</script>

<template>
  <div class="wrapper">
    <header>
      <img src="./assets/logo.svg" />
    </header>

    <div class="searchbox-wrapper">
      <input v-model="word" type="text" placeholder="Introduce una palabra en inglés..."
        @input="getData($event.target.value)" />
      <div class="icon-wrapper">
        <img src="./assets/icon-search.svg" class="lupa" />
      </div>
    </div>

    <div v-if="data?.length" class="word">
      <div class="word__title">
        <div>
          <h1> {{ data[0].word }} </h1>
          <h2>{{ data[0].phonetic }}</h2>
        </div>
        <img class="audio" :class="{ 'no-audio': !audioUrl }" src="./assets/icon-play.svg" @click="reproducirAudio()" />
      </div>

      <div v-for="meaning in data[0].meanings" :key="meaning.partOfSpeech" class="word__meanings">
        <div class="word__partOfSpeech">
          <h5>{{ meaning.partOfSpeech }}</h5>
          <div class="line"></div>
        </div>

        <div class="word__meaning">
          <h4>Meaning</h4>
          <br>
          <ul>
            <li v-for="definition in meaning.definitions">
              {{ definition.definition }}
            </li>
          </ul>
        </div>

        <div v-if="meaning.synonyms.length" class="word__synonyms">
          <h4>Synonyms</h4>
          <p>{{ meaning.synonyms.join(" ,") }}</p>
        </div>
      </div>

      <hr style="color: var(--clr-primary-400);">

      <footer v-if="data[0].sourceUrls.length">
        <p>Source</p>
        <a :href="data[0].sourceUrls[0]">
          {{ data[0].sourceUrls[0] }}
        </a>
      </footer>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  max-width: 600px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding-bottom: 4rem;
  padding: 0 1rem;
}

header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 0;
}

.searchbox-wrapper {
  position: relative;
}

.searchbox-wrapper input {
  width: 100%;
  padding: 1rem 0.85rem;
  border-radius: 1rem;
  border: none;
  background-color: var(--clr-primary-200);
  outline: none;
  font-size: 1rem;
  line-height: 1.25rem;
}

.icon-wrapper {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  padding: 0 1rem;
}

.audio {
  cursor: pointer;
}

.no-audio {
  opacity: 0.5;
  cursor: not-allowed;
}

.word__title {
  margin-left: auto;
  text-align: center;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin-bottom: 3rem;
  text-align: left;
}

.word__title h1 {
  font-size: 3rem;
  line-height: 1;
  color: var(--clr-primary-600);
}

.word__title h2 {
  margin-top: 0.5rem;
  font-weight: 400;
  color: var(--clr-secondary-100);
}

.word__meanings {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 2rem;
}

.word__partOfSpeech {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-style: italic;
}

.word__meaning h4 {
  color: var(--clr-primary-400);
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 400;
}

.word__meaning ul {
  list-style-type: disc;
  padding-left: 1rem;

}

.word__meaning li {
  margin: 0.75rem 0;
}

.word__meaning li::marker {
  color: var(--clr-secondary-100);
}

.word__synonyms {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 2rem;
}

.word__synonyms h4 {
  color: var(--clr-primary-400);
}

.word__synonyms p {
  color: var(--clr-secondary-100)
}

.line {
  height: 1px;
  width: 100%;
  background-color: var(--clr-primary-400);
}

footer {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-top: 2rem;
}

footer p {
  color: var(--clr-primary-400);
}

footer a {
  color: var(--clr-primary-700);
}
</style>
