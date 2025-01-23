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
      "https://jsonplaceholder.typicode.com/todos?_limit=0z"
    );
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
  <section>
    <div
      id="container"
      class="container mx-auto min-h-screen bg-center bg-no-repeat bg-cover p-4 relative"
    >
      <div
        class="absolute inset-0 bg-[url('https://www.worldatlas.com/r/w1300/upload/9e/9d/8a/chamonix-mont-blanc-resort-alps-france-iren-key.jpg')] bg-cover bg-center opacity-50"
        aria-hidden="true"
      ></div>
      <div
        class="relative z-10 flex flex-wrap justify-center items-center gap-8"
      >
        <div class="w-full lg:w-1/2 shadow-2xl">
          <div class="p-6 bg-white rounded-lg shadow-lg">
            <div class="flex justify-between items-center">
              <h2 class="text-3xl text-gray-800">Todolist</h2>
              <h2 class="text-3xl text-gray-800">Jindan</h2>
            </div>
            <p class="text-gray-600 my-4">Simple Todolist</p>
            <form @submit.prevent="addTask" class="flex gap-2">
              <input
                type="text"
                v-model="newTask"
                class="flex-1 border border-gray-300 text-slate-800 font-bold p-2 rounded focus:outline-none focus:ring focus:ring-blue-300"
                placeholder="Tambah Tugas"
              />
              <button
                type="submit"
                class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition"
              >
                Tambah
              </button>
            </form>
          </div>
        </div>

        <!-- Tugas List -->
        <div class="w-full lg:w-1/2 shadow-2xl">
          <div class="p-6 bg-white rounded-lg shadow-lg">
            <h2 class="text-3xl font-bold text-gray-800">Tugas</h2>
            <p class="text-gray-600 my-4">Tugas Anda:</p>
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
                  Hapus
                </button>
              </li>
            </ul>
            <div
              v-if="tasks.length === 0"
              class="text-center text-gray-500 mt-4"
            >
              Tidak ada Tugas
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style></style>
