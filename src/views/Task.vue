<template>
  <div class="task-view">
    <div class="t-task">
      <!-- <input
        type="text"
        class="task-title"
        :value="task[0].name"
        @change="updateTaskProperty($event, 'name')"
      />-->
      <div class="row">
        <b-icon icon="card-heading" scale="1.3" class="mt-2 ml-3" />
        <div>
          <input
            type="text"
            class="task-title ml-2"
            :value="task[0].name"
            @change="updateTaskProperty($event, 'name')"
          />
        </div>
      </div>
      <p class="list-desc">
        in list
        <span class="list-name">{{ task[1] }}</span>
      </p>
      <p class="mt-3">
        <span class="mr-3">
          <b-icon icon="justify-left" scale="1.3" />
        </span>Description
      </p>
      <textarea
        class="task-desc"
        :value="task[0].description"
        @change="updateTaskProperty($event, 'description')"
      />
    </div>
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
    },
    updateTaskProperty(e, key) {
      this.$store.commit("UPDATE_TASK", {
        task: this.task,
        key,
        value: e.target.value
      });
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
.task-title {
  padding: 0.25rem;
  width: 100%;
  display: block;
  font-weight: bold;
  font-size: 1.25rem;
  border: none;
  height: 2rem;
}
.task-title:focus,
.task-desc:focus {
  border: 2px solid #1190d0;
  border-radius: 0.25rem;
}
.task-desc {
  position: relative;
  width: 95%;
  background-color: #f1f1f1;
  padding: 0.25rem 0.5rem;
  border: 1px solid #f5f2f2;
  margin: 0.5rem;
  margin-left: 1.5rem;
  height: 10rem;
  line-height: 1.5;
  overflow-y: auto;
  border-radius: 0.25rem;
  margin-top: -0.2rem;
}
.task-desc:hover {
  background: #eceaea;
}
.list-desc {
  font-size: 0.8rem;
  padding-left: 2rem;
  padding-top: -1.3rem;
}
.list-name {
  text-decoration: underline;
}
</style>