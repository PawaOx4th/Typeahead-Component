<template>
  <div>
    <div class="flex flex-col pt-16 px-16">
      <h1 class=" mb-8 font-bold  text-6xl text-teal-400 font-mono ">Typeahead🔍 </h1>
      <div class="font-bold text-4xl">User ID: {{userId}}</div>
      <hr class="my-5">
      <input
        v-model="userData"
        ref="inputRef"
        type="text"
        class="input text-2xl border-2 p-3 w-8/12"
      >
    </div>
  </div>
</template>

<script>
import autocompleter from "autocompleter";
import data from "./data.json"; // cr :  // Data by https://jsonplaceholder.typicode.com/users

import "autocompleter/autocomplete.min.css"; // Default style sheet

// console.log(data.user);

// Edit data
const dataNew = data.user.map(({ id, name, username, email }) => {
  return {
    id,
    name,
    username,
    email,
    label: `${name}  / ${username}  / ${email}`
  };
});

export default {
  data() {
    return {
      userId: 0,
      userData: ""
    };
  },
  mounted() {
    // Test Ref
    // console.log(this.$refs.inputRef);
    this.autoCompleRef = autocompleter({
      input: this.$refs.inputRef,
      minLength: 1, // Min Value
      emptyMsg: "No Match Item Not found",
      fetch(text, update) {
        const pattern = new RegExp(text, "i");
        // console.log(text);
        // const search = [{ label: "Test" }]; Test
        const search = dataNew.filter(({ label }) => {
          return pattern.test(label);
        });
        update(search);
      },
      onSelect: ({ id, label }) => {
        this.userData = label;
        this.userId = id;
      }
    });
  },

  beforeDestroy() {
    this.autoCompleRef.destroy();
  }
};
</script>

<style scoped>
</style>