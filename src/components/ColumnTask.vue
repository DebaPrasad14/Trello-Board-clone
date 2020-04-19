<template>
  <div
    class="task"
    draggable
    @dragstart="pickupTask($event, taskId, colId)"
    @click="goToTask(task)"
    @dragover.prevent
    @dragenter.prevent
    @drop.stop="moveTaskOrColumn($event, column.tasks, colId, taskId)"
  >
    <span class="task-name">{{ task.name }}</span>
    <p class="task-name task-desc" v-if="task.description">{{ task.description }}</p>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      required: true
    },
    taskId: {
      type: Number,
      required: true
    },
    column: {
      type: Object,
      required: true
    },
    colId: {
      type: Number,
      required: true
    },
    board: {
      type: Object,
      required: true
    }
  },
  methods: {
    pickupTask(e, taskIndex, fromColumnIndex) {
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.dropEffect = "move";

      e.dataTransfer.setData("from-task-index", taskIndex);
      e.dataTransfer.setData("from-column-index", fromColumnIndex);
      e.dataTransfer.setData("type", "task");
    },
    goToTask(task) {
      this.$router.push({ name: "task", params: { id: task.id } });
    },
    moveTaskOrColumn(e, toTasks, toColumnIndex, toTaskIndex) {
      const type = e.dataTransfer.getData("type");
      if (type === "task") {
        this.moveTask(
          e,
          toTasks,
          toTaskIndex !== undefined ? toTaskIndex : toTasks.length
        );
      } else {
        this.moveColumn(e, toColumnIndex);
      }
    },
    moveTask(e, toTasks, toTaskIndex) {
      const fromColumnIndex = e.dataTransfer.getData("from-column-index");
      const fromTasks = this.board.columns[fromColumnIndex].tasks;
      const fromTaskIndex = e.dataTransfer.getData("from-task-index");

      this.$store.commit("MOVE_TASK", {
        fromTasks,
        fromTaskIndex,
        toTasks,
        toTaskIndex
      });
    },
    moveColumn(e, toColumnIndex) {
      const fromColumnIndex = e.dataTransfer.getData("from-column-index");
      this.$store.commit("MOVE_COLUMN", {
        fromColumnIndex,
        toColumnIndex
      });
    }
  }
};
</script>

<style scoped>
.task {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  margin-bottom: 0.2rem;
  padding: 0.5rem;
  border-radius: 0.25rem;
  background-color: #fff;
  color: #4a5568;
  cursor: pointer;
}
.task-name {
  width: 100%;
  font-weight: 500;
}
.task-desc {
  font-size: 13px;
  margin-top: 0.25rem;
}
p {
  margin-bottom: 0;
}
</style>