<script>
import taskCard from './components/taskCard.vue';
import taskInput from './components/taskInput.vue';
import { ref } from 'vue';

export default {
  name: 'App',
  components: {
    taskCard,
    taskInput
  },
  setup() {
    const defaultTaskList = ref([{
      id: 0, title: 'Задача 1', description: 'Описание 1', status: false
    }]);
    const taskListData = JSON.parse(localStorage.getItem('taskList')) || defaultTaskList;
    const taskList = ref(taskListData);
    const taskDoneClass = ref('task-done');

    const addTask = ({ title, description }) => {
      taskList.value = [...taskList.value, {id: taskList.value[taskList.value.length - 1].id+1, title, description, status: false}];
      saveData();
    }

    const doneTask = (id) => {
      taskList.value = taskList.value.map(x => {
        if(x.id === id)
          x.status = !x.status
        return x
      });
      saveData();
    }

    const saveEditTask = ({ id, newTitle, newDescription }) => {
      taskList.value = taskList.value.map(function (x) {
        if(x.id === id) {
          x.title = newTitle;
          x.description = newDescription;
        }
        return x
      });
      saveData();
    }

    const removeTask = (id) => {
      taskList.value = taskList.value.filter(x => x.id !== id);
      saveData();
    }

    const saveData = () => {
      const storageData = JSON.stringify(taskList.value);
      localStorage.setItem('taskList', storageData);
    }

    return {
      taskList,
      addTask,
      doneTask,
      saveEditTask,
      removeTask,
      saveData,
      taskDoneClass
    }
  }
}
</script>

<template>
  <section class="to-do-app">
    <header class="header">
      <h1>Задачи</h1>
      <taskInput @onAddTask="addTask"></taskInput>
    </header>
    <main class="main">
      <ul>
        <taskCard
          v-for="task in taskList"
          :key="task.id"
          :model="task"
          @onDone="doneTask(task.id)"
          @onSaveEdit="saveEditTask"
          @onRemove="removeTask(task.id)"
          :class="[task.status ? taskDoneClass : '']"
        ></taskCard>
      </ul>
    </main>
  </section>
</template>

<style>
.task-done {
  background-color: gainsboro;
  text-decoration: line-through;
  color: grey;
}
</style>