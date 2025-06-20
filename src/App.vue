<!-- <script setup>
import { RouterLink, RouterView } from 'vue-router'
import ChatBot from './views/ChatBot.vue'
</script>

<template>
  <div class="links">
    <RouterLink to="/" active-class="active">Home</RouterLink> |
    <RouterLink to="/about" active-class="active">About</RouterLink>
  </div>
  <RouterView />
</template>

<style scoped>
.active{
  color:white;
  background: green;
}

.links{
  text-align: center;
}
</style>

 -->




 <script setup>
 import { RouterLink, RouterView } from 'vue-router'
 import { ref } from 'vue'

 
 // To control the chatbot visibility
 const open = ref(false)
 </script>
 
 <template>
   <div>
     <!-- Navigation Links -->
     <div class="links">
       <RouterLink to="/" active-class="active">Home</RouterLink> |
       <RouterLink to="/about" active-class="active">About</RouterLink>
     </div>
 
     <!-- Main content view -->
     <RouterView />
 
     <!-- Chatbot Floating Widget -->
     <div class="chatbot-widget">
       <button @click="open = !open">💬</button> <!-- Toggle the chatbot visibility -->
       <ChatBot v-if="open" /> <!-- Show chatbot when 'open' is true -->
     </div>
   </div>
 </template>
 
 <style scoped>
 .active {
   color: white;
   background: green;
 }
 
 .links {
   text-align: center;
 }
 
 .chatbot-widget {
   position: fixed;
   bottom: 20px;
   right: 20px;
   z-index: 9999;
   background-color: #fff; /* Optional styling for the widget */
 }
 
 .chatbot-widget button {
   background-color: #ff6f61;
   color: white;
   padding: 10px;
   border-radius: 50%;
   cursor: pointer;
 }
 
 .chatbot-widget button:hover {
   background-color: #ff5733;
 }
 </style>
 