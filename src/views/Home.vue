<script setup lang="ts">
import { computed, reactive } from "vue";
import { useRoute } from "vue-router";
const route = useRoute();
const origin = route.query?.origin;
const partner = route.query?.partner;

// {
//         message: verificationStatus,
//         status: verificationStatus.toLowerCase(),
//         user: {
//           email: this.userEmail,
//           name: this.userFirstName + " " + this.userLastName,
//           crewUniqueId: this.crewUniqueId,
//           hasSubscription: this.crewHasSubscription,
//         },
//         formData: {
//           "crewpass-crew-status": verificationStatus,
//           "crewpass-crew-email": this.userEmail,
//           "crewpass-crew-crewUniqueId": this.crewUniqueId,
//           "crewpass-crew-name": this.userFirstName + " " + this.userLastName,
//         },
//       }

const inputItems = [
  "status",
  "email",
  "name",
  "crewUniqueId",
  "hasSubscription",
];

const data: any = reactive({
  inputs: {
    status: "pending",
    email: "alicia+test@nanonino.com",
    name: "Alicia Marin",
    crewUniqueId: "123456789",
    hasSubscription: true,
  },
});

const message = computed(() =>
  JSON.stringify({
    status: data.inputs.status,
    message: data.inputs.status,
    user: {
      email: data.inputs.email,
      name: data.inputs.name,
      crewUniqueId: data.inputs.crewUniqueId,
      hasSubscription: data.inputs.hasSubscription,
    },
    formData: {
      "crewpass-crew-status": data.inputs.status,
      "crewpass-crew-email": data.inputs.email,
      "crewpass-crew-crewUniqueId": data.inputs.crewUniqueId,
      "crewpass-crew-name": data.inputs.name,
    },
  })
);

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
      <h1 class="text-xl font-medium text-gray-700">Data from origin</h1>
      <div class="flex">Patner: {{ partner }}</div>
      <div class="flex">Origin: {{ origin }}</div>
    </div>
    <div class="flex w-full flex-col space-y-4 mt-16">
      <h1 class="text-xl font-medium text-gray-700">
        Post Message back to origin
      </h1>
      <div class="flex flex-col space-y-2 w-full">
        <div class="flex flex-col" v-for="item in inputItems">
          <label class="text-xs text-gray-400 ml-2 py-1" for="item">{{
            item
          }}</label>
          <input
            v-model="data.inputs[item]"
            type="text"
            class="p-2 rounded-2xl border-2 border-gray-100 w-full"
          />
        </div>
      </div>
      <pre class="bg-gray-100 p-2 rounded-3xl whitespace-pre-wrap">
      <code class="text-gray-800 text-xs">
        {{message}}
      </code>
    </pre>
      <textarea
        v-model="message"
        rows="6"
        class="p-4 rounded-3xl border-2 border-gray-200 w-full"
      />
      <button
        @click="sendMessage"
        class="p-4 rounded-3xl border-2 border-gray-200 hover:bg-gray-300 bg-gray-100"
      >
        Send
      </button>
    </div>
  </div>
</template>
