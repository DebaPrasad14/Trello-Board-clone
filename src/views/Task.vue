<template>
  <div class="task-view">
    <div class="t-task">{{ task.name }}</div>
    <div class="t-close" @click="closeModal">
      <b-icon icon="x" scale="1.4"></b-icon>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  computed: {
    ...mapGetters(["getTask"]),
    task() {
      return this.getTask(this.$route.params.id);
    }
  },
  methods: {
    closeModal() {
      this.$router.push({ name: "board" });
    }
  }
};
</script>

<style scoped>
.task-view {
  max-width: 700px;
  display: flex;
  flex-direction: row;
  background-color: #fff;
  position: relative;
  margin: 8rem;
  padding: 1rem 0;
  text-align: left;
  border-radius: 0.25rem;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin-right: auto;
  margin-left: auto;
}
.t-task {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  align-items: flex-start;
  justify-content: space-between;
  padding: 0 1rem;
}
.t-close {
  padding-right: 0.7rem;
  cursor: pointer;
  height: 25px;
  width: 25px;
  border-radius: 50%;
  padding: 2px 4px;
  margin: 0px 7px;
}
.t-close:hover {
  background-color: #d4d6da;
}
</style>