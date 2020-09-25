<template>
  <div class="flex flex-col items-center w-2/12 mx-auto">
    <h3 class="font-medium text-xl">Enter Github User Name</h3>
    <input
      v-model="name"
      class="ml-4 p-1 border-solid shadow-outline mt-4"
      placeholder="Github User name"
    />
    <button
      class="border-red-700 mt-4 w-32 rounded-md p-2 mb-10 uppercase font-semibold bg-blue-300 color-white border-solid"
      @click="newName = name"
    >Press me</button>
  </div>
  <ul>
    <li v-for="lib in data" :key="lib.name">{{lib.name}}</li>
  </ul>
</template>


<script>
import { ref, watch, reactive, toRefs } from "vue";
export default {
  name: "App",
  setup() {
    const name = ref(null);
    const newName = ref(null);
    const state = reactive({ data: [] });

    watch(() => {
      console.log("newName", newName.value);
      if (newName.value)
        fetch(`https://api.github.com/users/${newName.value}/repos`)
          .then((response) => response.json())
          .then((data) => {
            state.data = data;
            console.log("data", data);
            name.value = "";
          });
    });
    return {
      name,
      newName,
      ...toRefs(state),
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
