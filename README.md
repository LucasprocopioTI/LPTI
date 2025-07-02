<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>TaskFlow - Gerenciador de Tarefas</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f4;
    }
    header {
      background-color: #4a90e2;
      color: white;
      text-align: center;
      padding: 1rem 0;
    }
    main {
      max-width: 800px;
      margin: 2rem auto;
      padding: 1rem;
      background-color: white;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      border-radius: 8px;
    }
    #taskInput {
      width: 70%;
      padding: 0.5rem;
      margin-right: 0.5rem;
    }
    button {
      padding: 0.5rem 1rem;
      background-color: #4a90e2;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      background-color: #e2e2e2;
      padding: 0.75rem;
      margin: 0.5rem 0;
      border-radius: 4px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .status {
      margin-left: 1rem;
      font-size: 0.9rem;
      color: #333;
    }
    .actions button {
      margin-left: 0.5rem;
    }
  </style>
</head>
<body>
  <header>
    <h1>TaskFlow - Gerenciador de Tarefas</h1>
  </header>
  <main>
    <input type="text" id="taskInput" placeholder="Digite uma nova tarefa...">
    <button onclick="addTask()">Adicionar</button>
    <ul id="taskList"></ul>
  </main>

  <script>
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

    function saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    function renderTasks() {
      const taskList = document.getElementById('taskList');
      taskList.innerHTML = '';

      tasks.forEach((task, index) => {
        const li = document.createElement('li');
        li.innerHTML = `
          <span>${task.name}</span>
          <span class="status">[${task.status}]</span>
          <div class="actions">
            <button onclick="changeStatus(${index})">Próximo Status</button>
            <button onclick="deleteTask(${index})">Excluir</button>
          </div>
        `;
        taskList.appendChild(li);
      });
    }

    function addTask() {
      const taskInput = document.getElementById('taskInput');
      const name = taskInput.value.trim();
      if (name === '') return;

      tasks.push({ name, status: 'A Fazer' });
      taskInput.value = '';
      saveTasks();
      renderTasks();
    }

    function changeStatus(index) {
      const statusOrder = ['A Fazer', 'Em Progresso', 'Concluído'];
      const currentStatus = tasks[index].status;
      const nextIndex = (statusOrder.indexOf(currentStatus) + 1) % statusOrder.length;
      tasks[index].status = statusOrder[nextIndex];
      saveTasks();
      renderTasks();
    }

    function deleteTask(index) {
      tasks.splice(index, 1);
      saveTasks();
      renderTasks();
    }

    renderTasks();
  </script>
</body>
</html>
