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
    var uluru = { lat: -25.363, lng: 131.044 };
    var map = new google.maps.Map(document.getElementById("map"), {
      zoom: 4,
      center: uluru
    });

    var contentString =
      '<div id="content">' +
      '<div id="siteNotice">' +
      "</div>" +
      '<h1 id="firstHeading" class="firstHeading">Uluru</h1>' +
      '<div id="bodyContent">' +
      "<p><b>Uluru</b>, also referred to as <b>Ayers Rock</b>, is a large " +
      "sandstone rock formation in the southern part of the " +
      "Northern Territory, central Australia. It lies 335&#160;km (208&#160;mi) " +
      "south west of the nearest large town, Alice Springs; 450&#160;km " +
      "(280&#160;mi) by road. Kata Tjuta and Uluru are the two major " +
      "features of the Uluru - Kata Tjuta National Park. Uluru is " +
      "sacred to the Pitjantjatjara and Yankunytjatjara, the " +
      "Aboriginal people of the area. It has many springs, waterholes, " +
      "rock caves and ancient paintings. Uluru is listed as a World " +
      "Heritage Site.</p>" +
      '<p>Attribution: Uluru, <a href="https://en.wikipedia.org/w/index.php?title=Uluru&oldid=297882194" target=_blank>' +
      "https://en.wikipedia.org/w/index.php?title=Uluru</a> " +
      "(last visited June 22, 2009).</p>" +
      "</div>" +
      "</div>";

    var infowindow = new google.maps.InfoWindow({
      content: contentString
    });

    var marker = new google.maps.Marker({
      position: uluru,
      map: map,
      title: "Uluru (Ayers Rock)"
    });
    marker.addListener("click", function() {
      infowindow.open(map, marker);
    });
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
