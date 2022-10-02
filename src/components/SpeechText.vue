<script setup>
import { onMounted, ref } from "vue";

const speakBtnBool = ref(false);

const selectVoice = ref("default");
const textToSpeeach = ref("");

const synthesis = window.speechSynthesis;
let voices = ref([]);

onMounted(() => {
  voices.value = synthesis.getVoices();
  console.log(synthesis.getVoices());
});

function speakToText() {
  if (!voices.value) {
    voices.value = synthesis.getVoices();
  }

  if (!selectVoice) {
    return alert("Select voice");
  }

  if (!textToSpeeach) {
    return alert("Not void");
  }
  speakBtnBool.value = !speakBtnBool.value;

  const utterThis = new SpeechSynthesisUtterance(textToSpeeach.value);

  const nameSelectedVoice = selectVoice.value;
  const voiceSelect = voices.value.find(
    (voice) => voice.name == nameSelectedVoice
  );

  utterThis.voice = voiceSelect;

  synthesis.speak(utterThis);

  speakBtnBool.value = !speakBtnBool.value;
}
</script>

<template>
  <section class="flex flex-col gap-2">
    <select v-model="selectVoice" class="bg-gray-100 p-1 rounded">
      <option value="default">Voice default</option>
      <option v-for="voice in voices" :value="voice.name">
        {{ voice.name }}
      </option>
    </select>

    <textarea
      v-model="textToSpeeach"
      cols="30"
      rows="10"
      placeholder="Insert text"
      class="p-1 bg-gray-100 resize-none rounded"
    ></textarea>

    <button
      @click="speakToText"
      class="py-3 bg-gray-400 hover:bg-gray-600 rounded"
    >
      {{ speakBtnBool ? "Loading..." : "Speak" }}
    </button>
  </section>
</template>
