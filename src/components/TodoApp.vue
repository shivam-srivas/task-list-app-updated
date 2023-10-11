

<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Task List App</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="w-100 form-control"
      />
      <button class="btn btn-warning rounded-0" @click="submitTask">SUBMIT</button>
    </div>

    <!-- Filter dropdown -->
    <div class="flex mt-5">Filter: 
      <div>
      <select v-model="statusFilter" class="form-control">
        <option class="dropdown-toggl" value="all">All</option>
        <option value="to-do">To-Do</option>
        <option value="in-progress">In-Progress</option>
        <option value="finished">Finished</option>
      
      </select>
    </div>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-3">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in filteredTasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <div v-if="index === editedTask">
              <input
                v-model="tasks[index].status"
                @blur="updateStatus(index)"
                @keydown.enter="updateStatus(index)"
                :class="{
                  'text-danger': tasks[index].status === 'to-do',
                  'text-primary': tasks[index].status === 'finished',
                  'text-secondary': tasks[index].status === 'in-progress',
                }"
              />
            </div>
            <div v-else>
              <span
                class="pointer noselect"
                @click="editStatus(index)"
                :class="{
                  'text-danger': task.status === 'to-do',
                  'text-primary': task.status === 'finished',
                  'text-secondary': task.status === 'in-progress',
                }"
              >
                {{ task.status }}
              </span>
            </div>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "mytasklists",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Trip To Goa In October.",
          status: "to-do",
        },
        {
          name: "Create YouTube video.",
          status: "finished",
        },
        {
          name: "DSA 5 Ques Daily.",
          status: "to-do",
        },
        {
          name: "Study Full-Stack Dev 8 hours.",
          status: "in-progress",
        },
      ],
      statusFilter: 'all', // Default filter: show all tasks
    };
  },
  computed: {
    filteredTasks() {
      if (this.statusFilter === 'all') {
        return this.tasks;
      } else if (this.statusFilter === 'edited') {
        return this.tasks.filter(task => task.status === 'edited');
      } else {
        return this.tasks.filter(task => task.status === this.statusFilter);
      }
    },
  },
  methods: {
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    editStatus(index) {
      this.editedTask = index;
    },
    updateStatus(index) {
      this.editedTask = null;
      if (this.statuses.includes(this.tasks[index].status)) {
        this.tasks[index].status = this.tasks[index].status;
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      }
      this.task = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.line-through {
  text-decoration: line-through;
}
</style>
