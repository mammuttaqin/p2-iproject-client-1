<template>
  <div
    class="flex justify-center items-center h-screen bg-gradient-to-b from-blue-500 to-indigo-100"
  >
    <div class="flex flex-col">
      <p class="font-bold text-5xl m-3 text-indigo-900">
        {{ greetings.message }}
      </p>
      <div
        class="flex flex-col items-center bg-blue-500 rounded-xl border-blue-500"
      >
        <p class="text-white font-black text-2xl mt-2">Play as</p>
        <div class="flex flex-col mb-5 mt-2 items-center">
          <a
            class="text-white bg-blue-900 p-1 text-lg font-semibold rounded-lg w-3/4 flex justify-center cursor-pointer"
            @click.prevent="addPlayer('Guest')"
            >Guest</a
          >
          <p class="text-white">or</p>
          <form
            @submit.prevent="addPlayer(player)"
            class="flex flex-col items-center"
          >
            <button
              class="text-white bg-blue-900 p-1 text-lg font-semibold rounded-lg w-3/4 flex justify-center cursor-pointer"
              type="submit"
            >
              Login
            </button>
            <input
              class="mt-3 mb-1 rounded-lg h-10 p-2"
              type="text"
              name="player"
              placeholder="enter unique username"
              v-model="player"
            />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from "vuex";
export default {
  name: "Login",
  data() {
    return {
      player: ""
    };
  },
  computed: {
    ...mapState(["greetings"])
  },
  methods: {
    ...mapActions(["getStart", "getLogin"]),
    addPlayer(data) {
      this.getLogin(data)
        .then(() => {
          this.$socket.client.emit("listUsers", localStorage.username);
          this.$router.push({ name: "Home" });
          this.player = "";
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created() {
    this.getStart()
      .then(() => {})
      .catch(err => {
        console.log(err);
      });
  }
};
</script>

<style></style>
