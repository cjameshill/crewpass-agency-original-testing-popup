<script setup lang="ts">
import { ref, computed } from "vue";
import { useRoute } from "vue-router";
const route = useRoute();
const origin = route.query?.origin;
const partner = route.query?.partner;

const message = ref(JSON.stringify({ status: "pending" }));

const sendMessage = () => {
  if (!window.opener) {
    alert("Missing popup origin - window.opener");
  }
  try {
    window.opener.postMessage(message.value, origin);
  } catch (e: any) {
    console.log("error sending postMessage: ", e.message);
    alert(e.message || "error sending postMessage to origin");
  }
};
</script>
<template>
  <div class="w-full">
    <div class="flex flex-col space-y-4 my-8 py-4">
      <div class="flex">Patner: {{ partner }}</div>
      <div class="flex">Origin: {{ origin }}</div>
    </div>
    <div class="flex w-full flex-col space-y-4">
      <textarea
        v-model="message"
        rows="6"
        class="p-4 rounded-3xl border-2 border-gray-200 w-full"
      />
      <button
        @click="sendMessage"
        class="p-4 rounded-3xl border-2 border-gray-200 bg-gray-100"
      >
        Send
      </button>
    </div>
  </div>
</template>
