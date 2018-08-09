<template>
  <div class="home">
    <div id="map"></div>
    <h1>Add a task</h1>
    <input v-model="newTask.text">
    <button v-on:click="addTask()">Add task</button>

    <h1>Tasks ({{ numIncompleteTasks() }} remaining)</h1>
    <button v-on:click="deleteCompletedTasks()">Clear completed tasks</button>
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

#map {
  height: 300px;
}
</style>

<script>
/* global google */

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
  mounted: function() {
    // The location of Uluru
    var uluru = { lat: -25.344, lng: 131.036 };
    // The map, centered at Uluru
    var map = new google.maps.Map(document.getElementById("map"), {
      zoom: 4,
      center: uluru
    });
    // The marker, positioned at Uluru
    var marker = new google.maps.Marker({ position: uluru, map: map });
  },
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
    },
    deleteCompletedTasks: function() {
      var incompleteTasks = [];
      this.tasks.forEach(function(task) {
        if (!task.completed) {
          incompleteTasks.push(task);
        }
      });
      this.tasks = incompleteTasks;
    }
  },
  computed: {}
};
</script>
