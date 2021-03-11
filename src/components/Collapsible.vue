<template>
  <div>
    <button @click="setActive()" type="button" class="collapsible">
      <h3>{{ object.title }}</h3>
      <b>Created on:</b> {{ object.creation_date }} <b>Score:</b>
      {{ object.score }}
    </button>
    <div v-if="active" class="content">
      <h1>{{ object.title }}</h1>
      <span v-html="object.body"></span>
      <h3>Comments</h3>
      <div v-for="item in object.comments" :key="item">
        <b>Created on:</b> {{ item.creation_date }} <b>Score:</b>
        {{ item.score }} <b>Content: </b>
        <span v-html="item.body"></span>
      </div>

      <h2>Answers</h2>
      <div v-for="item in object.answers" :key="item">
        <b>Created on:</b> {{ item.creation_date }} <b>Score:</b>
        {{ item.score }} <b>Content: </b>
        <span v-html="item.body"></span>
        <h4>Answer Comments</h4>
        <div v-for="item in item.comments" :key="item">
          <b>Created on:</b> {{ item.creation_date }} <b>Score:</b>
          {{ item.score }} <b>Content: </b>
          <span v-html="item.body"></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Collapsible",
  data: function() {
    return { active: false };
  },
  props: {
    object: Object,
  },
  methods: {
    setActive() {
      console.log("Hitting this mf");
      this.active = !this.active;
    },
  },
};
</script>

<style>
/* Style the button that is used to open and close the collapsible content */
.collapsible {
  background-color: lightgrey;
  color: #444;
  cursor: pointer;
  padding: 18px;
  width: 100%;
  border: 3px solid grey;
  text-align: left;
  outline: none;
  font-size: 15px;
}

/* Add a background color to the button if it is clicked on (add the .active class with JS), and when you move the mouse over it (hover) */
.active,
.collapsible:hover {
  background-color: lightblue;
}

/* Style the collapsible content. Note: hidden by default */
.content {
  padding: 20px;
  margin: 20px;

  background-color: lightcyan;
  width: 100%;
}
</style>
