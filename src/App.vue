<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-4" v-for="statusCard in statusCards" :key="statusCard.status">
          <StatusCard 
            :title="statusCard.title" 
            :titleClasses="statusCard.titleClasses" 
            :newTask="statusCard.newTasks" 
            :status="statusCard.status" 
            :tasks="filteredTasks(statusCard.status)"
            @new-task="addTask"
            @status-updated="updateStatus"
            />
      </div>
    </div>
  </div>
</template>


<script>
import StatusCard from './components/StatusCard.vue';
import logger from "./mixins/logger";

export default {
  name: "App",
  mixins: [logger],
  components: {
    StatusCard
  },
  provide() {
    return {
      maxNumbersOfChars: 255,
    }
  },
  data() {
    return {
      tasks: [
        {
          id: 1,
          content: "Dashboard überarbeiten.",
          status: 1,
        },
        {
          id: 2,
          content: "Anwendung auf Vue.js umstellen.",
          status: 0,
        },
      ],
      statusCards: [
        {
          title: "Neue Aufgaben", 
          titleClasses: "bg-secondary text-white",
          newTasks: true,
          status: 0,
        },
        {
          title: "in Bearbeitung", 
          titleClasses: "bg-primary text-white",
          newTasks: false,
          status: 1,
        }, 
        {
          title: "Erledigt", 
          titleClasses: "bg-success text-white",
          newTasks: false,
          status: 2,
        }
      ]
    };
  },
  methods: {
    filteredTasks(status) {
      return this.tasks.filter((task) => task.status === status)
    },
    addTask(task) {
      this.tasks.id = Math.random();
      this.tasks.push(task)
    },
    updateStatus(status) {
      console.log(this.status)
      const task = this.tasks.find(
        (task) => task.id === Number(status.taskId));
      task.status = status.newStatus
    }
  }
};
</script>

<style>
@import "~bootstrap/dist/css/bootstrap.min.css";
</style>
