<template>
  <div
    class="t-column"
    draggable
    @drop="moveTaskOrColumn($event, column.tasks, colId)"
    @dragover.prevent
    @dragenter.prevent
    @dragstart.self="pickupColumn($event, colId)"
  >
    <div class="t-title">{{ column.name }}</div>
    <div class="task-wrapper">
      <ColumnTask
        v-for="(task, taskId) in column.tasks"
        :key="taskId"
        :task="task"
        :taskId="taskId"
        :column="column"
        :colId="colId"
        :board="board"
      />
      <CreateTask :column="column" :colId="colId" />
    </div>
  </div>
</template>

<script>
import ColumnTask from "@/components/ColumnTask";
import CreateTask from "@/components/CreateTask";

export default {
  props: {
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
  components: {
    ColumnTask,
    CreateTask
  },
  methods: {
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
    },
    pickupColumn(e, fromColumnIndex) {
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.dropEffect = "move";

      e.dataTransfer.setData("from-column-index", fromColumnIndex);
      e.dataTransfer.setData("type", "column");
    }
  }
};
</script>

<style scoped>
.t-column {
  background: #dee5ec;
  text-align: left;
  padding: 0.5rem;
  margin-right: 1rem;
  min-width: 350px;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  border-radius: 0.25rem;
}
.t-title {
  display: flex;
  align-items: center;
  margin-bottom: 0.5rem;
  font-weight: bold;
}
</style>