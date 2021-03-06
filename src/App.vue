<template>
  <div>
    <input v-model="tag" class="form-control" required />
    <button @click="searchTag(tag)">
      Look up tag!
    </button>
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    searchTag(tag) {
      console.log(tag);
      this.axios
        .get(
          "https://api.stackexchange.com/2.2/questions?site=stackoverflow&tagged=" +
            encodeURIComponent(tag)
        )
        .then((response) => console.log(response));
    },
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
