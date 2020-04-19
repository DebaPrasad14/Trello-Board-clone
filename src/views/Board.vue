<template>
  <div>
    <navbar />
    <div class="board">
      <div class="t-row">
        <BoardColumn
          v-for="(column,colId) in board.columns"
          :key="colId"
          :column="column"
          :colId="colId"
          :board="board"
        />
        <CreateColumn />
      </div>
      <div class="task-modal" v-if="isTaskOpen" @click.self="close">
        <router-view />
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
import BoardColumn from "@/components/BoardColumn";
import CreateColumn from "@/components/CreateColumn";
import { mapState } from "vuex";

export default {
  components: {
    Navbar,
    BoardColumn,
    CreateColumn
  },
  computed: {
    ...mapState(["board"]),
    isTaskOpen() {
      return this.$route.name === "task";
    }
  },
  methods: {
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
.task-modal {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
}
</style>
