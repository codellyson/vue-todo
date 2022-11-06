<script setup lang="ts">
import { ref } from "vue";

type tasksType = { id: number; task: string; completed: boolean };

const tasks = ref<tasksType[]>([]);
const taskInput = ref("");

const setTaskInput = (value: string) => (taskInput.value = value);

function addTask() {
  tasks.value.push({
    id: Math.floor(Math.random() * 10),
    task: taskInput.value,
    completed: false,
  });
  taskInput.value = "";
}

function removeHandler(_task: number) {
  const filteredTasks = tasks.value.filter(({ id }) => id !== _task);
  tasks.value = filteredTasks;
}

function statusHandler(event: any, _task: number) {
  const selectedTask: any = tasks.value.find(({ id }) => id === _task);
  selectedTask.completed = event.target.checked;
  const inde = tasks.value.indexOf(selectedTask);
  tasks.value.splice(inde, 1, selectedTask);
}
</script>

<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="./assets/logo.svg"
      width="125"
      height="125"
    />
  </header>

  <main>
    <div>
      <input
        placeholder="Type in your tasks"
        @input="(e) => setTaskInput(e.target?.value as any)"
        :value="taskInput"
      />
      <button @click="addTask">add task</button>
    </div>
    <div>
      <p style="margin: 0">Task lists - ({{ tasks.length }})</p>
      <small
        >Number of completed tasks - ({{
          tasks.filter((task) => task.completed).length
        }})</small
      >
      <ul v-if="tasks.length > 0" style="margin-top: 1rem">
        <li v-for="task in tasks" v-bind:key="task.task">
          <span
            :style="task.completed ? 'text-decoration: line-through' : ''"
            >{{ task.task }}</span
          >
          &nbsp;
          <input
            type="checkbox"
            :name="task.task"
            id=""
            @input="(e) => statusHandler(e, task.id)"
          />
          <span style="color: red" @click="removeHandler(task.id)">X</span>
        </li>
      </ul>
      <div v-else style="margin-top: 1rem">No tasks yet!!</div>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
