<script>
// @ts-nocheck

  import { onMount } from 'svelte';

  let tasks = [];
  let newTaskTitle = '';
  let newTaskDescription = '';
  let newTaskDate = '';

 
  onMount(() => {
    const savedTasksString = localStorage.getItem('tasks');
    const savedTasks = savedTasksString ? JSON.parse(savedTasksString) : [];
    tasks = savedTasks;
  });

  function addTask() {
   
    if (newTaskTitle.trim() === '' || newTaskDescription.trim() === '' || newTaskDate.trim() === '') return;

  
    tasks = [...tasks, { title: newTaskTitle, description: newTaskDescription, date: newTaskDate, completed: false }];
    
  
    newTaskTitle = '';
    newTaskDescription = '';
    newTaskDate = '';
    saveTasks();
  }

  function removeTask(index) {
    tasks = tasks.filter((_, i) => i !== index);
    saveTasks();
  }

  function toggleTask(index) {
    tasks[index].completed = !tasks[index].completed;
    saveTasks();
  }

  function saveTasks() {
    localStorage.setItem('tasks', JSON.stringify(tasks));
  }
</script>

<style>

::placeholder{
  display: flex;
  text-align: center;
}

input{

  border-radius: 6px;
}

  ul {
    list-style-type: none;
    padding: 0;
    display: flex; 
    flex-wrap: wrap; 
    gap: 10px; 
    border-radius: 15px;
  }
  li {
    display: flex; 
    align-items: center; 
    padding: 8px;
    border-radius: 20px;
    gap: 10px;
  }

  button {
    margin-left: auto;
    cursor: pointer;
  }
</style>



<div>
  <input
    type="text"
    placeholder="Titre de la tâche"
    bind:value={newTaskTitle}
    on:keydown={(e) => e.key === 'Enter' && addTask()}
  />
  <input
    type="text"
    placeholder="Description de la tâche"
    bind:value={newTaskDescription}
    on:keydown={(e) => e.key === 'Enter' && addTask()}
  />
  <input
    type="date"
    bind:value={newTaskDate}
    on:keydown={(e) => e.key === 'Enter' && addTask()}
  />
  <button on:click={addTask}>Ajouter Tâche</button>
</div>

<ul>
  {#each tasks as task, index}
    <li>
  
      <p>{task.title}</p>
      <p>{task.description}</p>
      <p>{task.date}</p>
      <button on:click={() => removeTask(index)}>Supprimer</button>
    </li>
  {/each}
</ul>
