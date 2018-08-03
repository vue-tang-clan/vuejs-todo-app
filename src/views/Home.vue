<template>
  <div class="home">
    <h1>Add a task</h1>
    <input v-model="newTask.text">
    <button v-on:click="addTask()">Add task</button>

    <h1>Tasks ({{ numIncompleteTasks() }} remaining)</h1>
    <ul>
      <li v-for="task in tasks" v-on:click="completeTask(task)">
        <span v-bind:class="{completed: task.completed}">{{ task.text }}</span>
      </li>
    </ul>
  </div>
</template>

<style>
.completed {
  text-decoration: line-through;
}
</style>

<script>
export default {
  data: function() {
    return {
      tasks: [
        { id: 1, text: "Take out the garbage", completed: false },
        { id: 2, text: "Make the bed", completed: false },
        { id: 3, text: "Mow the lawn", completed: false }
      ],
      newTask: { text: "", completed: false }
    };
  },
  created: function() {},
  methods: {
    addTask: function() {
      if (this.newTask.text) {
        this.tasks.push(this.newTask);
        this.newTask = { text: "", completed: false };
      }
    },
    completeTask: function(inputTask) {
      // var index = this.tasks.indexOf(inputTask);
      // this.tasks.splice(index, 1);
      inputTask.completed = !inputTask.completed;
    },
    numIncompleteTasks: function() {
      var count = 0;
      this.tasks.forEach(function(task) {
        if (!task.completed) {
          count += 1;
        }
      });
      return count;
    }
  },
  computed: {}
};
</script>
