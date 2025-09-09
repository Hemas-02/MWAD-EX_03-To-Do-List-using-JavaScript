# MWAD-EX_03-To-Do-List-using-JavaScript
## Date: 9/09/2025

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hemalatha's Modal Todo</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }

    body {
      min-height: 100vh;
      background: linear-gradient(135deg, #fce4ec, #e3f2fd, #e8f5e9);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
    }

    header {
      margin-top: 20px;
      font-size: 2rem;
      font-weight: bold;
      color: #444;
      text-align: center;
    }

    .task-list {
      width: 90%;
      max-width: 500px;
      margin: 20px auto;
      background: rgba(255,255,255,0.8);
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.1);
    }

    .task {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px;
      margin: 8px 0;
      background: white;
      border-radius: 10px;
      transition: 0.3s;
    }

    .task:hover { transform: scale(1.02); }

    .task.completed span {
      text-decoration: line-through;
      color: gray;
    }

    .task span {
      flex: 1;
      margin-left: 10px;
    }

    .task button {
      border: none;
      padding: 6px 10px;
      border-radius: 6px;
      margin-left: 6px;
      cursor: pointer;
      color: white;
      font-size: 0.9rem;
    }

    .delete { background: #e57373; }
    .edit { background: #ffb74d; }

    #openModalBtn {
      margin: 20px;
      padding: 12px 20px;
      background: linear-gradient(135deg, #ba68c8, #64b5f6);
      color: white;
      border: none;
      border-radius: 12px;
      font-size: 1rem;
      cursor: pointer;
      transition: 0.3s;
    }

    #openModalBtn:hover { transform: scale(1.05); }

    /* Modal Styling */
    .modal {
      display: none;
      position: fixed;
      z-index: 10;
      left: 0; top: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.5);
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .modal-content {
      background: white;
      padding: 20px;
      border-radius: 15px;
      width: 90%;
      max-width: 400px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
      text-align: center;
    }

    .modal-content input {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 8px;
      border: 2px solid #ccc;
    }

    .modal-content button {
      padding: 10px 15px;
      border: none;
      border-radius: 10px;
      margin: 5px;
      cursor: pointer;
      font-weight: bold;
    }

    .save-btn { background: #81c784; color: white; }
    .close-btn { background: #e57373; color: white; }

    footer {
      background: rgba(0,0,0,0.6);
      color: white;
      width: 100%;
      text-align: center;
      padding: 15px;
      font-size: 1rem;
    }

    footer b { color: #fff176; }
  </style>
</head>
<body>
  <header>🎀 Hemalatha's Todo App</header>

  <button id="openModalBtn">+ Add Task</button>

  <div class="task-list" id="taskList"></div>

  <!-- Modal -->
  <div class="modal" id="taskModal">
    <div class="modal-content">
      <h3>Add New Task</h3>
      <input type="text" id="taskInput" placeholder="Enter your task...">
      <br>
      <button class="save-btn" id="saveTaskBtn">Save</button>
      <button class="close-btn" id="closeModalBtn">Cancel</button>
    </div>
  </div>

  <footer>
    Developed by <b>Hemalatha R</b> | Reg.No: <b>212224040114</b>
  </footer>

  <script>
    const taskList = document.getElementById("taskList");
    const openModalBtn = document.getElementById("openModalBtn");
    const taskModal = document.getElementById("taskModal");
    const closeModalBtn = document.getElementById("closeModalBtn");
    const saveTaskBtn = document.getElementById("saveTaskBtn");
    const taskInput = document.getElementById("taskInput");

    let tasks = JSON.parse(localStorage.getItem("modalTasks")) || [];

    function saveTasks() {
      localStorage.setItem("modalTasks", JSON.stringify(tasks));
    }

    function renderTasks() {
      taskList.innerHTML = "";
      tasks.sort((a,b) => a.completed - b.completed); // completed at bottom
      tasks.forEach((task, index) => {
        const div = document.createElement("div");
        div.className = "task" + (task.completed ? " completed" : "");
        div.innerHTML = `
          <input type="checkbox" ${task.completed ? "checked" : ""}>
          <span>${task.text}</span>
          <button class="edit">✎</button>
          <button class="delete">🗑</button>
        `;

        // Toggle complete
        div.querySelector("input").addEventListener("change", () => {
          tasks[index].completed = !tasks[index].completed;
          saveTasks();
          renderTasks();
        });

        // Edit task
        div.querySelector(".edit").addEventListener("click", () => {
          const newText = prompt("Edit task:", task.text);
          if (newText) {
            tasks[index].text = newText;
            saveTasks();
            renderTasks();
          }
        });

        // Delete task
        div.querySelector(".delete").addEventListener("click", () => {
          tasks.splice(index, 1);
          saveTasks();
          renderTasks();
        });

        taskList.appendChild(div);
      });
    }

    openModalBtn.addEventListener("click", () => {
      taskModal.style.display = "flex";
      taskInput.value = "";
      taskInput.focus();
    });

    closeModalBtn.addEventListener("click", () => {
      taskModal.style.display = "none";
    });

    saveTaskBtn.addEventListener("click", () => {
      const text = taskInput.value.trim();
      if (text) {
        tasks.push({ text, completed: false });
        saveTasks();
        renderTasks();
        taskModal.style.display = "none";
      } else {
        alert("Please enter a task!");
      }
    });

    window.addEventListener("click", (e) => {
      if (e.target == taskModal) taskModal.style.display = "none";
    });

    renderTasks();
  </script>
</body>
</html>


## OUTPUT
<img width="1912" height="1019" alt="Screenshot 2025-09-09 134801" src="https://github.com/user-attachments/assets/b7dc22e0-466a-41b5-9b72-58a3a455d588" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
