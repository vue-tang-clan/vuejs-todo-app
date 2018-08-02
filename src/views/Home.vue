<template>
  <div class="home">
    <h1>Tasks</h1>
    <ul>
      <li v-for="task in tasks">
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<style>
</style>

<script>
export default {
  data: function() {
    return {
      tasks: [
        { id: 1, text: "Take out the garbage", completed: false },
        { id: 2, text: "Make the bed", completed: false },
        { id: 3, text: "Mow the lawn", completed: false }
      ]
    };
  },
  created: function() {},
  methods: {},
  computed: {}
};
</script>
