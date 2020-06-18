<template>
  <div>
    <h1 class="m-4 text-2xl font-bold">New session</h1>
    <input type="text" class="m-4 text-xl bg-blue-200" placeholder="Name" />
    <div class="p-4">
      <span class="font-bold">Song name</span> <br />
      <span class="">Artist</span>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, onMounted, onUnmounted } from "vue";
import firebase from "firebase/app";
import "firebase/auth";
import "firebase/database";
import router from "@/router";

async function useSession() {
  const user = firebase.auth().currentUser;
  if (!user) return;
  const uid = user.uid;
  const defaultDatabase = firebase.database();
  const reference = defaultDatabase.ref(`users/${uid}/session`);
  function onSessionCallback(dataSnapshot: firebase.database.DataSnapshot) {
    console.log("on data", dataSnapshot.toJSON());
    /* do nothing yet*/
  }

  onMounted(() => {
    reference.on("value", onSessionCallback);
  });

  onUnmounted(() => {
    reference.off("value", onSessionCallback);
  });
  return {};
}

export default defineComponent({
  setup() {
    async function logout() {
      await firebase.auth().signOut();
      router.push("/");
    }

    console.log("foo");
    console.log("foobar");

    return { logout, ...useSession() };
  }
});
</script>
