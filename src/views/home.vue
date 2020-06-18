<template>
  <div class="p-12">
    <h1 class="text-center text-2xl font-bold">Auxless spotify</h1>
    <div class="flex justify-center">
      <button
        @click="startSession"
        class="bg-blue-500 hover:bg-blue-600 m-4 mr-12 text-white p-4 rounded-full"
      >
        Start session
      </button>
      <button
        class="bg-gray-300 hover:bg-gray-400 p-4 m-4 rounded-full"
        @click="() => (isClosed = true)"
      >
        Join session
      </button>
    </div>
    <div
      v-if="isClosed"
      class="fixed bg-black bg-opacity-25 inset-0"
      @click="isClosed = !isClosed"
    >
      <div class="mt-32 mx-auto p-4 bg-white max-w-md" @click.stop>
        <span>Enter session code</span>
        <br />
        <input type="number" class="bg-gray-300" />
        <button class="bg-blue-500 text-white px-2 m-2" @click="joinSession">
          Go
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import router from "@/router";
import store from "@/store";

export default defineComponent({
  name: "Home",
  setup() {
    function startSession() {
      const spotifyLinked = store.getters.spotifyLinked();
      if (spotifyLinked) return router.push("/session");
      router.push({ path: "/connect", query: { returnUrl: "/session" } });
    }
    function joinSession() {
      console.log("clicked the join settion button");
    }
    return { startSession, joinSession, isClosed: false };
  }
});
</script>
