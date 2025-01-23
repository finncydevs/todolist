<script setup>
import { ref, onMounted } from "vue";

const tasks = ref([]);
const newTask = ref("");

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({ id: Date.now(), title: newTask.value.trim() });
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

async function getTask() {
  try {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/todos?_limit=5"
    ); // Fetch only 5 tasks
    const data = await response.json();
    tasks.value = data.map((task) => ({
      id: task.id,
      title: task.title,
      completed: task.completed,
    }));
  } catch (error) {
    console.error(error);
  }
}

onMounted(async () => {
  await getTask();
});
</script>

<template>
  <div
    id="container"
    class="container mx-auto min-h-screen bg-cover bg-center p-4"
  >
    <div class="flex flex-wrap justify-center items-center gap-8">
      <div class="w-full lg:w-1/2">
        <div class="p-6 bg-white rounded-lg shadow-lg">
          <h2 class="text-3xl text-gray-800">Todolist</h2>
          <p class="text-gray-600 my-4">A simple todolist app</p>
          <form @submit.prevent="addTask" class="flex gap-2">
            <input
              type="text"
              v-model="newTask"
              class="flex-1 border border-gray-300 text-slate-800 text-bold p-2 rounded focus:outline-none focus:ring focus:ring-blue-300"
              placeholder="Add new task"
            />
            <button
              type="submit"
              class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition"
            >
              Add Task
            </button>
          </form>
        </div>
      </div>

      <!-- Task List Section -->
      <div class="w-full lg:w-1/2">
        <div class="p-6 bg-white rounded-lg shadow-lg">
          <h2 class="text-3xl font-bold text-gray-800">Tasks</h2>
          <p class="text-gray-600 my-4">Your tasks:</p>
          <ul class="space-y-2">
            <li
              v-for="(task, index) in tasks"
              :key="task.id"
              class="flex justify-between items-center text-slate-800 bg-gray-100 p-2 rounded"
            >
              <span>{{ task.title }}</span>
              <button
                @click="deleteTask(index)"
                class="text-red-500 hover:text-red-700"
              >
                Delete
              </button>
            </li>
          </ul>
          <div v-if="tasks.length === 0" class="text-center text-gray-500 mt-4">
            No tasks available
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
#container {
  background-image: url("https://www.worldatlas.com/r/w1300/upload/9e/9d/8a/chamonix-mont-blanc-resort-alps-france-iren-key.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  filter: grayscale(0.5);
}
</style>
