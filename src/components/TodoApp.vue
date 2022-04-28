<template>
  <div class="container">
    <h2 class="text-center mt-5">Vue Todo App</h2>
    <!-- Input -->
    <div class="d-flex">
      <input
        v-model="nameTask"
        type="text"
        placeholder="enter task"
        class="form-control"
      />
      <button
        @click="submitTask"
        class="btn btn-warning rounded-0"
        style="margin-left: 5px"
      >
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-3">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" class="text-center">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === availableStatuses[2] }">
              {{ task.name }}
            </span>
          </td>
          <td class="text-center">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === availableStatuses[0],
                'text-primary': task.status === availableStatuses[1],
                'text-success': task.status === availableStatuses[2],
              }"
            >
              {{ firstCharUpper(task.status) }}
            </span>
          </td>
          <td>
            <div @click="editTask(index)" class="text-center">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div @click="removeTask(index)" class="text-center">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },
  data() {
    return {
      nameTask: "",
      editedTaskIndex: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Steal bananas from the store.",
          status: "to-do",
        },
        {
          name: "Eat 1kg chocolate in 1 hour.",
          status: "in-progress",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.nameTask.length === 0) return;
      if (this.editedTaskIndex === null) {
        this.tasks.push({
          name: this.nameTask,
          status: "to-do",
        });
      } else {
        this.tasks[this.editedTaskIndex].name = this.nameTask;
      }
      this.editedTaskIndex = null;
      this.nameTask = "";
    },
    editTask(index) {
      this.editedTaskIndex = index;
      this.nameTask = this.tasks[index].name;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex++];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
