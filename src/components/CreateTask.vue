<template>
  <div v-if="isTaskClicked && columnId === colId">
    <textarea
      type="text"
      class="input-box"
      v-model="taskTitle"
      placeholder="Enter a title for this card"
      @keyup.enter="createTask(taskTitle, column.tasks)"
    />
    <button type="button" class="btn add-task" @click="createTask(taskTitle, column.tasks)">Add card</button>
    <button type="button" class="btn cancel-task" @click="cancelTask">&times;</button>
  </div>
  <div v-else>
    <button
      type="button"
      class="add-card"
      @click="openInputBox(colId)"
      v-if="column.tasks.length > 0"
    >
      <b-icon icon="plus" scale="1.2" class="mr-1" />Add another card
    </button>
    <button type="button" class="add-card" @click="openInputBox(colId)" v-else>
      <b-icon icon="plus" scale="1.2" class="mr-1" />Add a card
    </button>
  </div>
</template>

<script>
export default {
  props: {
    column: {
      type: Object,
      required: true
    },
    colId: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      isTaskClicked: false,
      taskTitle: "",
      columnId: null
    };
  },
  methods: {
    createTask(title, tasks) {
      if (title.trim().length === 0) return;
      this.$store.commit("CREATE_TASK", {
        tasks,
        name: title
      });
      this.taskTitle = "";
    },
    cancelTask() {
      this.isTaskClicked = !this.isTaskClicked;
      this.taskTitle = "";
    },
    openInputBox(id) {
      this.columnId = id;
      this.isTaskClicked = !this.isTaskClicked;
    }
  }
};
</script>

<style scoped>
.input-box {
  display: block;
  padding: 0.3rem;
  width: 100%;
  background-color: #fff;
  border: none;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  border-radius: 0.25rem;
  color: #4a5568;
  padding: 0.5rem;
}
.add-task {
  background-color: #9abfe2;
  font-size: 15px;
  margin-bottom: none;
  color: #4a5568;
  padding: 0.25rem 0.7rem;
}
.cancel-task {
  font-size: 30px;
  margin-bottom: none;
  padding: 0 15px;
  color: #4a5568;
  border: none;
}
.add-card {
  display: block;
  padding: 0.3rem;
  width: 100%;
  background-color: transparent;
  border: none;
  text-align: left;
  font-size: 15px;
  margin-top: 0.5rem;
  color: #797d7f;
  border-radius: 0.25rem;
}
.add-card:hover {
  background-color: #d5dbdb;
}
button:focus,
button:active {
  outline: none;
  box-shadow: none;
}
</style>