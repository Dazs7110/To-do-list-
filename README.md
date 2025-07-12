HTML
<link rel="stylesheet" href="style.css" />
<audio id="sound" src="sound.mp3" preload="auto"></audio>

  <div class="container">
    <h1>📅 Todoリスト</h1>
   <form id="todo-form">
  <input type="text" id="todo-input" placeholder="やることを入力" />
  <input type="date" id="todo-date" />
  <button type="submit">追加</button>
</form>
    <ul id="todo-list"></ul>
  </div>

  <script src="script.js"></script>

  CSS
  body {
  font-family: 'Arial', sans-serif;
  background: #f2f2f2;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 500px;
  margin: 50px auto;
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
}

form {
  display: flex;
  gap: 10px;
  margin-bottom: 1rem;
}

input {
  flex: 1;
  padding: 0.5rem;
  font-size: 16px;
}

button {
  padding: 0.5rem 1rem;
  font-size: 16px;
  background: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  padding: 0.5rem;
  background: #eee;
  margin-bottom: 0.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li.done {
  text-decoration: line-through;
  color: gray;
}
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

li.animate {
  animation: fadeIn 0.3s ease forwards;
}

JAVA SCRIPT
 const form = document.getElementById("todo-form");
const input = document.getElementById("todo-input");
const dateInput = document.getElementById("todo-date");
const list = document.getElementById("todo-list");

window.addEventListener("DOMContentLoaded", loadTodos);

form.addEventListener("submit", function (e) {
  e.preventDefault();
  const text = input.value.trim();
  const date = dateInput.value;
  if (text === "") return;

  const todo = {
    text,
    date,
    done: false
  };

  addTodoToDOM(todo);
  saveTodo(todo);
  // 音を再生
const sound = document.getElementById("sound");
if (sound) sound.play();

  input.value = "";
  dateInput.value = "";
});

function addTodoToDOM(todo) {
  const li = document.createElement("li");

  const taskText = document.createElement("span");
  taskText.textContent = todo.text;

  const dueDate = document.createElement("small");
  if (todo.date) {
    dueDate.textContent = `📅 ${todo.date}`;
  }

  li.appendChild(taskText);
  if (todo.date) li.appendChild(dueDate);

  if (todo.done) li.classList.add("done");

  li.addEventListener("click", () => {
    li.classList.toggle("done");
    updateLocalStorage();
  });

  li.addEventListener("contextmenu", (e) => {
    e.preventDefault();
    li.remove();
    updateLocalStorage();
  });

  list.appendChild(li);
}

function saveTodo(todo) {
  const todos = getTodos();
  todos.push(todo);
  localStorage.setItem("todos", JSON.stringify(todos));
}

function updateLocalStorage() {
  const lis = document.querySelectorAll("#todo-list li");
  const todos = Array.from(lis).map(li => {
    const span = li.querySelector("span");
    const small = li.querySelector("small");
    return {
      text: span.textContent,
      date: small ? small.textContent.replace("📅 ", "") : "",
      done: li.classList.contains("done")
    };
  });
  localStorage.setItem("todos", JSON.stringify(todos));
}

function getTodos() {
  return JSON.parse(localStorage.getItem("todos")) || [];
}

function loadTodos() {
  const todos = getTodos();
  todos.forEach(addTodoToDOM);
}
