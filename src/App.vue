<template>
  <div class="todo-list">
    <h1>Daftar Mobil</h1>
    <div class="input-container">
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Masukkan nama mobil">
      <button @click="addTask" class="add-button">Tambah</button>
    </div>
    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index">
        <div class="task-item">
          <input type="checkbox" v-model="task.completed" class="checkbox-left">
          <span 
            v-if="!task.isEditing" 
            :class="{ 'completed': task.completed }" 
            class="task-title"
          >
            {{ task.title }}
          </span>
          <input 
            v-else 
            v-model="task.title" 
            @keyup.enter="disableEditing(task)" 
            @blur="disableEditing(task)" 
            class="task-title-input"
          >
          <button @click="removeTask(index)" class="delete-button">Hapus</button>
          <button @click="enableEditing(task)" class="edit-button" v-if="!task.isEditing">Edit</button>
          <button @click="disableEditing(task)" class="edit-button" v-if="task.isEditing">Simpan</button>
        </div>
      </li>
    </ul>
    <div>
      <label>
        <input type="checkbox" v-model="showOnlyIncomplete">
        Tampilkan mobil yang masih ada
      </label>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      showOnlyIncomplete: false
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ title: this.newTask.trim(), completed: false, isEditing: false });
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    enableEditing(task) {
      task.isEditing = true;
    },
    disableEditing(task) {
      if (task.title.trim() === '') {
        // If the title is empty, remove the task
        this.tasks = this.tasks.filter(t => t !== task);
      } else {
        task.isEditing = false;
      }
    }
  },
  computed: {
    filteredTasks() {
      if (this.showOnlyIncomplete) {
        return this.tasks.filter(task => !task.completed);
      } else {
        return this.tasks;
      }
    }
  }
};
</script>

<style scoped>

.todo-list {
  border: 1px solid rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.568);
  margin: auto;
  width: 500px;
  padding: 25px;
  border-radius: 10px;
  border-color: rgb(0, 0, 0);
}

.input-container {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

input {
  flex-grow: 1;
  padding: 5px;
  margin-right: 10px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

.add-button {
  background-color: green;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
  border-radius: 4px;
}

ul {
  list-style-type: none;
  padding: 10px;
}

.completed {
  text-decoration: line-through;
}

.delete-button {
  background-color: red;
  color: white;
  border: none;
  padding: 10px 10px;
  cursor: pointer;
  border-radius: 4px;
}

.edit-button {
  background-color: blue;
  color: white;
  border: none;
  padding: 10px 10px;
  cursor: pointer;
  border-radius: 4px;
  margin-left: 5px;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.checkbox-left {
  margin-right: 10px;
}

.task-title {
  flex-grow: 1;
  cursor: pointer;
}

.task-title-input {
  flex-grow: 1;
  padding: 5px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

</style>
