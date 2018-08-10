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
/* global geocoder, google  */

export default {
  data: function() {
    return {
      tasks: [
        { id: 1, text: "Take out the garbage", completed: false },
        { id: 2, text: "Make the bed", completed: false },
        { id: 3, text: "Mow the lawn", completed: false }
      ],
      places: [
        {
          // lat: 37.773972,
          // lng: -122.431297,
          address: "San Francisco",
          description:
            "<h1>A city in California</h1><p>Click <a>here</a> to learn more"
        },
        {
          // lat: 41.881832,
          // lng: -87.623177,
          address: "Chicago",
          description: "A city in Illinois"
        },
        {
          // lat: 40.73061,
          // lng: -73.935242,
          address: "New York City",
          description: "A city in New York"
        }
      ],
      newTask: { text: "", completed: false }
    };
  },
  mounted: function() {
    var map = new google.maps.Map(document.getElementById("map"), {
      zoom: 4,
      center: this.places[1]
    });
    var geocoder = new google.maps.Geocoder();

    this.places.forEach(function(place) {
      var address = place.address;
      geocoder.geocode({ address: address }, function(results, status) {
        if (status === "OK") {
          map.setCenter(results[0].geometry.location);
          var infowindow = new google.maps.InfoWindow({
            content: place.description
          });
          var marker = new google.maps.Marker({
            map: map,
            position: results[0].geometry.location,
            title: place.address
          });
          marker.addListener("click", function() {
            infowindow.open(map, marker);
          });
        } else {
          alert(
            "Geocode was not successful for the following reason: " + status
          );
        }
      });

      // var infowindow = new google.maps.InfoWindow({
      //   content: place.description
      // });
      // var marker = new google.maps.Marker({
      //   position: place,
      //   map: map,
      //   title: place.address
      // });
      // marker.addListener("click", function() {
      //   infowindow.open(map, marker);
      // });
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
