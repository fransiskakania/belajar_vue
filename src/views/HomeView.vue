<script>
import { ref, onMounted, computed } from 'vue';
import { url } from '../source.jsx';

export default {
  name: 'HomeView',
  setup() {
    const todos = ref([]);
    const searchQuery = ref('');

    onMounted(async () => {
      const response = await fetch(url); // Use the imported url constant
      todos.value = await response.json();
    });

    const filteredTodos = computed(() => {
      return todos.value.filter(todo => 
        todo.title.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    });

    return {
      todos,
      searchQuery,
      filteredTodos
    };
  }
};
</script>

<template>
  <main>
    <div class="search-container">
      <input v-model="searchQuery" placeholder="Search by title" />
      <span class="search-icon"> </span>
    </div>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Title</th>
          <th>Completed</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="todo in filteredTodos" :key="todo.id">
          <td data-label="ID">{{ todo.id }}</td>
          <td data-label="Title">{{ todo.title }}</td>
          <td data-label="Completed">{{ todo.completed ? 'Yes' : 'No' }}</td>
        </tr>
      </tbody>
    </table>
  </main>
</template>

<style scoped>
main {
  /* display: flex; */
  flex-direction: column;
  align-items: center;
  padding: 70px;
  width: 200%; /* Full width of the container */
  box-sizing: border-box; /* Include padding and border in the element's total width and height */
}

.search-container {
  display: inline-block;
  align-items: center;
  margin-bottom: 20px;
  width: 5000px;
  max-width: 1100px;
  position: reltive;
}

.search-container input {
  width: 100%;
  padding: 10px 40px 10px 10px; /* Adjust padding to make space for the icon */
  box-sizing: border-box;
  border: 2px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.search-container .search-icon {
  /* position: absolute; */
  right: 10px;
  font-size: 20px;
  color: #aaa;
}

.search-container input:focus {
  border-color: #007BFF;
  outline: none;
}

table {
  width: 1100px; /* Full width */
  border-collapse: collapse;
  /* margin: 20px 0; Adjusted margin */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #f8f9fa;
  animation: fadeIn 1s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

th, td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left; /* Added text alignment */
  transition: background-color 0.3s ease, color 0.3s ease;
}

th {
  background-color: #007BFF; /* Updated header color */
  color: white; /* Updated text color */
}

th, td {
  border-radius: 5px;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #007BFF; /* Hover color for rows */
  color: white; /* Hover text color for rows */
}

td[data-label="ID"] {
  width: 10%; /* Misalnya, 10% untuk kolom ID */
}

td[data-label="Title"] {
  width: 60%; /* Misalnya, 60% untuk kolom Title */
}

td[data-label="Completed"] {
  width: 30%; /* Misalnya, 30% untuk kolom Completed */
}

td[data-label="Title"] {
  padding-left: 20px;
  transition: padding-left 0.3s ease;
}

td[data-label="Title"]:hover {
  padding-left: 30px; /* Increased padding on hover */
}

tr:hover td[data-label="Title"] {
  padding-left: 30px; /* Consistent padding on hover */
}

tr:hover td {
  color: white; /* Ensure all text is white on hover */
}
</style>