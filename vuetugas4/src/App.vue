<script setup>
import { ref, reactive } from 'vue'; // Mengimpor ref dan reactive untuk data reaktif

// Impor gambar lokal dari folder assets
import image1 from '@/assets/Image1.jpeg';
import image2 from '@/assets/Image2.jpeg';
import image3 from '@/assets/Image3.jpeg';

// Data reaktif untuk kalkulator
const calculatorData = reactive({
  num1: 0,
  num2: 0,
  result: 0,
});

function calculate(operation) {
  if (operation === 'add') {
    calculatorData.result = calculatorData.num1 + calculatorData.num2;
  } else if (operation === 'subtract') {
    calculatorData.result = calculatorData.num1 - calculatorData.num2;
  } else if (operation === 'multiply') {
    calculatorData.result = calculatorData.num1 * calculatorData.num2;
  } else if (operation === 'divide') {
    calculatorData.result = calculatorData.num2 !== 0 ? calculatorData.num1 / calculatorData.num2 : 'Tidak bisa dibagi dengan nol';
  }
}

// Data reaktif untuk daftar tugas (todo list)
const todoList = ref([]);
const newTodo = ref('');

function addTodo() {
  if (newTodo.value) {
    todoList.value.push({ text: newTodo.value, completed: false });
    newTodo.value = ''; // Reset input setelah menambah
  }
}

function toggleTodoCompletion(index) {
  todoList.value[index].completed = !todoList.value[index].completed; // Toggle status selesai
}

function deleteTodo(index) {
  todoList.value.splice(index, 1); // Menghapus item dari todo list
}

// Data reaktif untuk carousel gambar
const currentImageIndex = ref(0);
const images = [
  { src: image1, description: 'Gambar Pertama' },
  { src: image2, description: 'Gambar Kedua' },
  { src: image3, description: 'Gambar Ketiga' },
];

function nextImage() {
  currentImageIndex.value = (currentImageIndex.value + 1) % images.length;
}

function prevImage() {
  currentImageIndex.value = (currentImageIndex.value - 1 + images.length) % images.length;
}
</script>

<template>
  <div>
    <!-- Bagian Kalkulator -->
    <section>
      <h2>Kalkulator Sederhana</h2>
      <input v-model.number="calculatorData.num1" type="number" placeholder="Angka 1" />
      <input v-model.number="calculatorData.num2" type="number" placeholder="Angka 2" />
      <div>
        <button @click="calculate('add')">Tambah</button>
        <button @click="calculate('subtract')">Kurangi</button>
        <button @click="calculate('multiply')">Kalikan</button>
        <button @click="calculate('divide')">Bagi</button>
      </div>
      <p>Hasil: {{ calculatorData.result }}</p>
    </section>

    <!-- Bagian Daftar Tugas -->
    <section>
      <h2>Daftar Tugas</h2>
      <input v-model="newTodo" type="text" placeholder="Tambahkan tugas baru" />
      <button @click="addTodo">Tambah Tugas</button>
      <ul>
        <li v-for="(todo, index) in todoList" :key="index">
          <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }">{{ todo.text }}</span>
          <button @click="toggleTodoCompletion(index)">
            {{ todo.completed ? 'Belum Selesai' : 'Selesai' }}
          </button>
          <button @click="deleteTodo(index)">Hapus</button>
        </li>
      </ul>
    </section>

    <!-- Bagian Carousel Gambar -->
    <section>
      <h2>Carousel Gambar</h2>
      <div class="carousel">
        <button @click="prevImage">Sebelumnya</button>
        <div>
          <img :src="images[currentImageIndex].src" alt="Gambar Carousel" />
          <p>{{ images[currentImageIndex].description }}</p>
        </div>
        <button @click="nextImage">Berikutnya</button>
      </div>
    </section>
  </div>
</template>

<style scoped>
div {
  text-align: center;
  margin-top: 2rem;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  cursor: pointer;
  margin: 5px;
}

.carousel {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
}

.carousel img {
  width: 400px;
  height: 400px;
  object-fit: cover;
  margin: 0 10px;
}
</style>
