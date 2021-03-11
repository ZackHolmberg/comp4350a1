<template>
  <div>
    <input v-model="tag" class="form-control" required />
    <button class="searchBtn" @click="searchTag(tag)">
      Look up tag!
    </button>
  </div>
  <br /><br />
  <div v-for="item in toDisplay" :key="item">
    <Collapsible :object="item" />
  </div>

  <h3 style="color:darkgreen;">Time taken: {{ timeTaken }} seconds</h3>
</template>

<script>
import Collapsible from "./components/Collapsible.vue";

export default {
  name: "App",
  components: {
    Collapsible,
  },
  data: function() {
    return { toDisplay: [], timeTaken: 0 };
  },
  methods: {
    searchTag(tag) {
      // Get a date object for the current time
      var d = new Date();

      // Set it to one month ago
      d.setDate(d.getDay() - 7);

      var unix = Math.round(+d / 1000);

      let mostVoted = [];
      let newest = [];
      let i;

      const t0 = performance.now();

      this.axios
        .get(
          "https://api.stackexchange.com/2.2/search?fromdate=" +
            unix +
            "&order=desc&sort=creation&tagged=" +
            encodeURIComponent(tag) +
            "&site=stackoverflow&filter=!)c5QMjAfN_fpNUj3OeFii.L4mQGv4nt0aIVq6goV3lhcH"
        )
        .then((response) => {
          let toFilter = response.data.items;
          for (i = 0; i < 10 && i < toFilter.length; i++) {
            newest[i] = toFilter[i];
          }

          this.axios
            .get(
              "https://api.stackexchange.com/2.2/search?fromdate=" +
                unix +
                "&order=desc&sort=votes&tagged=" +
                encodeURIComponent(tag) +
                "&site=stackoverflow&filter=!)c5QMjAfN_fpNUj3OeFii.L4mQGv4nt0aIVq6goV3lhcH"
            )
            .then((response) => {
              let toFilter = response.data.items;
              var i;
              for (i = 0; i < 10 && i < toFilter.length; i++) {
                mostVoted[i] = toFilter[i];
              }

              // ===========================================
              // ADD TO THE TODISPLAY LIST
              // ===========================================

              for (i = 0; i < 10 && i < mostVoted.length; i++) {
                let temp = mostVoted[i];

                const found = this.toDisplay.some(
                  (el) => el.question_id === temp.question_id
                );
                if (!found) {
                  this.toDisplay[i] = mostVoted[i];
                }
              }

              const currLength = mostVoted.length;
              for (i = 0; i < 10 && i < newest.length; i++) {
                let temp = newest[i];

                const found = this.toDisplay.some(
                  (el) => el.question_id === temp.question_id
                );
                if (!found) {
                  this.toDisplay[i + currLength] = newest[i];
                }
              }

              // ===========================================
              // SORT LIST IN DESCENDING ORDER
              // ===========================================

              this.toDisplay.sort(
                (a, b) =>
                  parseFloat(b.creation_date) - parseFloat(a.creation_date)
              );

              // ===========================================
              // CHANGE DATE TO SOMETHING HUMAN READABLE
              // ===========================================

              for (i = 0; i < this.toDisplay.length; i++) {
                const creationDate = new Date(
                  this.toDisplay[i].creation_date * 1000
                ).toLocaleString();
                this.toDisplay[i].creation_date = creationDate;
              }

              console.log("To Display:", this.toDisplay);
              const t1 = performance.now();

              this.timeTaken = (t1 - t0) / 1000;
            });
        });
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

body {
  background-color: darkgrey;
}

input {
  border-radius: 25px;
  width: 5%;
  margin-right: 25px;
  padding: 10px;
  border: 2px solid black;
}

.searchBtn {
  border-radius: 25px;
  border: 2px solid black;
  width: 100px;
  padding: 10px;
  background-color: lightgoldenrodyellow;
  cursor: pointer;
}
</style>
