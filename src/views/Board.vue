<template>
  <div>
    <navbar />
    <div class="board">
      <!-- {{ board}} -->
      <div class="t-row">
        <div class="t-column" v-for="(column,idx) in board.columns" :key="idx">
          <div class="t-title">{{ column.name }}</div>
          <div class="task-wrapper">
            <div class="task" v-for="(task,idx) in column.tasks" :key="idx" @click="goToTask(task)">
              <span class="task-name">{{ task.name }}</span>
              <p class="task-name task-desc" v-if="task.description">{{ task.description }}</p>
            </div>
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
  background: #cbd5e0;
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
.task-modal {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
}
</style>