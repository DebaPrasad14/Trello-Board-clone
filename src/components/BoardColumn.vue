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
import MovingTaskAndColumnMixin from "@/mixins/MovingTaskAndColumnMixin";

export default {
  components: {
    ColumnTask,
    CreateTask
  },
  mixins: [MovingTaskAndColumnMixin],
  methods: {
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