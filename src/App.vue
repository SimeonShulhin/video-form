<script setup>
import { ref } from 'vue';
import RadioLabel from './components/RadioLabel.vue';

const step = ref(1);
const playbackSpeed = ref(1.2);
const visualStyle = ref('colorfulComics');
const captionStyle = ref('colorfulComics');

const language = ref('');
const scriptType = ref('');
const topic = ref('');
const script = ref('');
const voiceOver = ref('');
const backgroundMusic = ref('');

const tabClass = (stepNumber) => {
  return [
    'px-4 py-2 rounded-md',
    step.value === stepNumber ? 'bg-white shadow font-bold' : 'text-gray-500',
  ];
};

const isFormValid = () => {
  return (
    language.value !== '' &&
    scriptType.value !== '' &&
    topic.value !== '' &&
    script.value !== '' &&
    visualStyle.value !== '' &&
    voiceOver.value !== '' &&
    backgroundMusic.value !== '' &&
    captionStyle.value !== ''
  );
};
</script>

<template>
  <div class="container mx-auto p-4">
    <!-- Title -->
    <h2 class="text-2xl font-semibold mb-4 text-center">Create Your Video</h2>

    <!-- Tabs -->
    <div class="flex justify-center mb-4 bg-gray-100 rounded-md shadow">
      <button :class="tabClass(1)" @click="step = 1">Language</button>
      <button :class="tabClass(2)" @click="step = 2">Script</button>
      <button :class="tabClass(3)" @click="step = 3">Video Settings</button>
      <button :class="tabClass(4)" @click="step = 4">Generate</button>
    </div>

    <!-- Content Container -->
    <div class="bg-white rounded-lg shadow p-6 mb-6">
      <!-- Step 1: Language -->
      <div v-if="step === 1">
        <label class="block mb-2">Select Language</label>
        <select v-model="language" class="block w-full p-2 border rounded">
          <option value="">Choose a language</option>
          <option value="english">English</option>
          <option value="spanish">Spanish</option>
        </select>
      </div>

      <!-- Step 2: Script -->
      <div v-if="step === 2">
        <label class="block mb-2 required">Choose script type</label>
        <select v-model="scriptType" class="block w-full p-2 border rounded">
          <option value="">Choose a script type</option>
          <option value="type1">Type 1</option>
          <option value="type2">Type 2</option>
        </select>
        <label class="block mt-4 mb-2 required">Enter your topic</label>
        <input
          type="text"
          v-model="topic"
          class="block w-full p-2 border rounded required"
          placeholder="Enter your topic" />
        <button class="bg-black text-white px-4 py-2 rounded mt-4">Generate Script</button>
        <label class="block mt-4 mb-2 required">Script</label>
        <textarea
          v-model="script"
          class="block w-full p-2 border rounded mt-4"
          placeholder="Generated script will appear here"></textarea>
      </div>

      <!-- Step 3: Video Settings -->
      <div v-if="step === 3">
        <label class="block mb-2 required">Visual Style</label>
        <div class="grid grid-cols-2 gap-4">
          <label
            class="border rounded p-2 cursor-pointer"
            :class="{
              checked: visualStyle === 'colorfulComics',
            }">
            <input
              type="radio"
              name="visualStyle"
              value="colorfulComics"
              v-model="visualStyle"
              class="hidden"
              @click="() => console.log('click')" />
            Colorful Comics
          </label>
          <RadioLabel name="visualStyle" value="clipart" v-model="visualStyle">
            Clipart
          </RadioLabel>
          <RadioLabel name="visualStyle" value="cinematic" v-model="visualStyle">
            Cinematic
          </RadioLabel>
          <RadioLabel name="visualStyle" value="pixelArt" v-model="visualStyle">
            Pixel Art
          </RadioLabel>
          <RadioLabel name="visualStyle" value="anime" v-model="visualStyle"> Anime </RadioLabel>
        </div>
        <label class="block mt-4 mb-2 required">Voice Over</label>
        <select v-model="voiceOver" class="block w-full p-2 border rounded">
          <option value="">Select voiceover</option>
          <option value="voice1">Voice 1</option>
          <option value="voice2">Voice 2</option>
        </select>
        <label class="block mt-4 mb-2 required">Background Music</label>
        <select v-model="backgroundMusic" class="block w-full p-2 border rounded">
          <option value="">Select background music</option>
          <option value="music1">Music 1</option>
          <option value="music2">Music 2</option>
        </select>
        <label class="block mt-4 mb-2">Playback Speed</label>
        <input
          type="range"
          min="0.5"
          max="2"
          step="0.1"
          class="block w-full slider-custom"
          v-model="playbackSpeed" />
        <p class="text-center">{{ playbackSpeed }}x</p>
        <label class="block mt-4 mb-2 required">Caption Style</label>
        <div class="grid grid-cols-2 gap-4">
          <RadioLabel name="captionStyle" value="simple" v-model="captionStyle">
            Simple
          </RadioLabel>
          <RadioLabel name="captionStyle" value="fancy" v-model="captionStyle"> Fancy </RadioLabel>
          <RadioLabel name="captionStyle" value="neon" v-model="captionStyle"> Neon </RadioLabel>
          <RadioLabel name="captionStyle" value="noCaptions" v-model="captionStyle">
            No Captions
          </RadioLabel>
        </div>
      </div>

      <!-- Step 4: Generate video -->
      <div v-if="step === 4" class="text-center">
        <button class="bg-black text-white px-4 py-2 rounded" :disabled="!isFormValid()">
          Generate Video
        </button>
        <p class="text-red-500 my-2" v-if="!isFormValid()">
          Please fill in all required fields before generating the video.
        </p>
        <p class="text-gray-500">This may take a few minutes. Please don't close the browser.</p>
      </div>
    </div>

    <!-- Previous/Next Buttons Outside the Form Container -->
    <div class="flex justify-between">
      <button :disabled="step === 1" @click="step--" class="step-button">Previous</button>
      <button :disabled="step === 4" @click="step++" class="step-button">Next</button>
    </div>
  </div>
</template>

<style scoped>
.step-button {
  @apply bg-black text-white px-4 py-2 rounded;
}

button[disabled] {
  @apply bg-gray-300 px-4 py-2 rounded;
}
.slider-custom {
  width: 100%;
  height: 8px;
  border-radius: 4px;
  accent-color: #1f2937;
}

.required:after {
  content: '!';
  width: 15px;
  height: 15px;
  border: 1px solid red;
  color: red;
  border-radius: 50%;
  display: inline-block;
  align-items: center;
  justify-content: center;
  font-size: 10px;
  font-weight: bold;
  text-align: center;
  margin-left: 0.5rem;
}

.checked {
  @apply bg-blue-500 text-white;
}
</style>
