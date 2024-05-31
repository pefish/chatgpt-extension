<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

const openAiApiKey = import.meta.env["VITE_OPENAI_API_KEY"]
console.log("openAiApiKey", openAiApiKey)
const query = ref<string>('');
const response = ref<string>('');

const askChatGPT = async () => {
  try {
    const result = await axios.post(
      'https://api.openai.com/v1/chat/completions',
      {
        model: 'gpt-3.5-turbo',
        messages: [{ role: 'user', content: query.value }],
        max_tokens: 50
      },
      {
        headers: {
          'Authorization': `Bearer ${openAiApiKey}`,
          'Content-Type': 'application/json'
        }
      }
    );
    response.value = result.data.choices[0].message.content;
  } catch (error: any) {
    console.error('Error: ', error);
    response.value = `Error: ${error.response.status} ${error.response.statusText}`;
  }

};
</script>

<template>
  <div id="app">
    <h1>ChatGPT Extension</h1>
    <input v-model="query" placeholder="Ask ChatGPT..." />
    <button @click="askChatGPT">Send</button>
    <p>{{ response }}</p>
  </div>
</template>

<style scoped></style>
