<template>
  <div>
    <navbar />
    <div class="board">
      <div class="t-row">
        <div
          class="t-column"
          v-for="(column,colId) in board.columns"
          :key="colId"
          draggable
          @drop="moveTaskOrColumn($event, column.tasks, colId)"
          @dragover.prevent
          @dragenter.prevent
          @dragstart.self="pickupColumn($event, colId)"
        >
          <div class="t-title">{{ column.name }}</div>
          <div class="task-wrapper">
            <div
              class="task"
              v-for="(task,taskId) in column.tasks"
              :key="taskId"
              draggable
              @dragstart="pickupTask($event, taskId, colId)"
              @click="goToTask(task)"
            >
              <span class="task-name">{{ task.name }}</span>
              <p class="task-name task-desc" v-if="task.description">{{ task.description }}</p>
            </div>
            <div v-if="isClicked && columnId === colId">
              <textarea
                type="text"
                class="input-box"
                v-model="taskTitle"
                placeholder="Enter a title for this card"
                @keyup.enter="createTask(taskTitle, column.tasks)"
              />
              <button
                type="button"
                class="btn add-task"
                @click="createTask(taskTitle, column.tasks)"
              >Add card</button>
              <button type="button" class="btn cancel-task" @click="cancelTask">&times;</button>
            </div>
            <button
              type="button"
              class="add-card"
              @click="openInputBox(colId)"
              v-else
            >+ Add another card</button>
          </div>
        </div>
      </div>
      <div class="task-modal" v-if="isTaskOpen" @click.self="close">
        <router-view />
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
import { mapState } from "vuex";

export default {
  components: {
    Navbar
  },
  data() {
    return {
      isClicked: false,
      columnId: null,
      taskTitle: ""
    };
  },
  computed: {
    ...mapState(["board"]),
    isTaskOpen() {
      return this.$route.name === "task";
    }
  },
  methods: {
    goToTask(task) {
      this.$router.push({ name: "task", params: { id: task.id } });
    },
    close() {
      this.$router.push({ name: "board" });
    },
    createTask(title, tasks) {
      if (title.trim().length === 0) return;
      this.$store.commit("CREATE_TASK", {
        tasks,
        name: title
      });
      this.reset();
    },
    openInputBox(id) {
      this.columnId = id;
      this.isClicked = !this.isClicked;
    },
    cancelTask() {
      this.isClicked = !this.isClicked;
      this.reset();
    },
    pickupTask(e, taskIndex, fromColumnIndex) {
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.dropEffect = "move";

      e.dataTransfer.setData("task-index", taskIndex);
      e.dataTransfer.setData("from-column-index", fromColumnIndex);
      e.dataTransfer.setData("type", "task");
    },
    pickupColumn(e, fromColumnIndex) {
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.dropEffect = "move";

      e.dataTransfer.setData("from-column-index", fromColumnIndex);
      e.dataTransfer.setData("type", "column");
    },
    moveTaskOrColumn(e, toTasks, toColumnIndex) {
      const type = e.dataTransfer.getData("type");
      if (type === "task") {
        this.moveTask(e, toTasks);
      } else {
        this.moveColumn(e, toColumnIndex);
      }
    },
    moveTask(e, toTasks) {
      console.log("dp", this.board);
      const fromColumnIndex = e.dataTransfer.getData("from-column-index");
      const fromTasks = this.board.columns[fromColumnIndex].tasks;
      const taskIndex = e.dataTransfer.getData("task-index");

      this.$store.commit("MOVE_TASK", {
        fromTasks,
        toTasks,
        taskIndex
      });
    },
    moveColumn(e, toColumnIndex) {
      const fromColumnIndex = e.dataTransfer.getData("from-column-index");
      this.$store.commit("MOVE_COLUMN", {
        fromColumnIndex,
        toColumnIndex
      });
    },
    reset() {
      this.taskTitle = "";
    }
  }
};
</script>

<style scoped>
.board {
  background-color: #9abfe2;
  height: 90vh;
  overflow: auto;
  padding: 0.5rem 1rem;
}
.t-row {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
}
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
.task {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  margin-bottom: 0.25rem;
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
.task-modal {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
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
}
.cancel-task {
  font-size: 30px;
  margin-bottom: none;
  padding: 0 15px;
  color: #4a5568;
}
</style>
