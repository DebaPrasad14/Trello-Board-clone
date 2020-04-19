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
import MovingTaskAndColumnMixin from "@/mixins/MovingTaskAndColumnMixin";

export default {
  mixins: [MovingTaskAndColumnMixin],
  props: {
    task: {
      type: Object,
      required: true
    },
    taskId: {
      type: Number,
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