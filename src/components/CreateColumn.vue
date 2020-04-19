<template>
  <div class="add-column">
    <div v-if="isAddListClicked" class="list-form-container">
      <div class="list-form">
        <input
          type="text"
          class="column-input"
          placeholder="Enter list title..."
          v-model="newColumnName"
          @keyup.enter="createColumn"
        />
      </div>
      <div class="list-btn-container">
        <button type="button" class="btn add-task" @click="createColumn()">Add List</button>
        <button type="button" class="btn cancel-task" @click="cancelColumn">&times;</button>
      </div>
    </div>
    <button type="button" class="btn add-list-btn" @click="addNewList" v-else>
      <b-icon icon="plus" scale="1.0" class="mr-1" />Add another list
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isAddListClicked: false,
      newColumnName: ""
    };
  },
  methods: {
    createColumn() {
      if (this.newColumnName.trim().length === 0) return;
      this.$store.commit("CREATE_COLUMN", {
        name: this.newColumnName
      });
      this.newColumnName = "";
    },
    cancelColumn() {
      this.isAddListClicked = !this.isAddListClicked;
      this.newColumnName = "";
    },
    addNewList() {
      this.isAddListClicked = !this.isAddListClicked;
    }
  }
};
</script>

<style scoped>
.list-form-container {
  display: block;
  background: #dee5ec;
  border-radius: 0.25rem;
  padding: 0.3rem;
  padding-bottom: 0.5rem;
  margin-right: 1rem;
}
.column-input {
  padding: 0.5rem;
  flex-grow: 1;
  height: 2rem;
  width: 100%;
  border: 2px solid #1190d0;
  border-radius: 0.25rem;
}
.list-btn-container {
  text-align: left;
  height: 2.4rem;
  padding-top: 0;
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
}
.add-list-btn {
  text-align: left;
  margin-right: 1rem;
  border: 1px solid #979797;
  height: 2.3rem;
  border-radius: 0.25rem;
  background-color: #797d7f;
  color: #fff;
  opacity: 0.6;
}
.add-list-btn:hover {
  opacity: 0.5;
}
button:focus,
button:active {
  outline: none;
  box-shadow: none;
}
.add-column {
  display: flex;
  flex-direction: column;
  min-width: 350px;
  border-radius: 0.25rem;
  margin-right: 1rem;
  background-color: transparent;
}
</style>