<template>
    <div class="chatbot">
      <div class="messages">
        <div v-for="(msg, i) in messages" :key="i" :class="msg.sender">{{ msg.text }}</div>
      </div>
      <input v-model="userInput" @keyup.enter="sendMessage" placeholder="Ask me anything..." />
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  import axios from 'axios';
  
  const messages = ref([]);
  const userInput = ref('');
  const router = useRouter();
  
  const sendMessage = async () => {
    if (!userInput.value.trim()) return;
  
    const input = userInput.value.trim();
    messages.value.push({ text: input, sender: 'user' });
  
    try {
      const res = await axios.post(
        'https://dialogflow.googleapis.com/v2/projects/YOUR_PROJECT_ID/agent/sessions/123456789:detectIntent',
        {
          queryInput: {
            text: {
              text: input,
              languageCode: 'en'
            }
          }
        },
        {
          headers: {
            Authorization: `Bearer YOUR_ACCESS_TOKEN`,
            'Content-Type': 'application/json'
          }
        }
      );
  
      const fulfillment = res.data.queryResult.fulfillmentMessages;
  
      for (const msg of fulfillment) {
        if (msg.payload && msg.payload.navigate_to) {
          const path = msg.payload.navigate_to;
          messages.value.push({ text: `Navigating to ${path}...`, sender: 'assistant' });
          router.push(path);
          return;
        } else if (msg.text) {
          messages.value.push({ text: msg.text.text[0], sender: 'assistant' });
        }
      }
  
      userInput.value = '';
    } catch (err) {
      messages.value.push({ text: "Dialogflow error. Check console.", sender: 'assistant' });
      console.error(err);
    }
  };
  </script>
  
  <style scoped>
  .chatbot {
    max-width: 400px;
    margin: auto;
    border: 1px solid #ddd;
    padding: 10px;
  }
  .messages {
    height: 250px;
    overflow-y: auto;
    margin-bottom: 10px;
  }
  .user {
    text-align: right;
    color: blue;
  }
  .assistant {
    text-align: left;
    color: green;
  }
  input {
    width: 100%;
    padding: 10px;
  }
  </style>
  