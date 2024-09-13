<script>
	import Header from './Header.svelte';
	import Modal from './Modal.svelte';
	import { onMount } from 'svelte';
	import moment from 'moment';
  
	let modalIsActived = false;
	let tasks = [];
	let completedTasks = [];
  
	function actived() {
	  modalIsActived = !modalIsActived;
	  document.body.style.overflow = modalIsActived ? 'hidden' : 'auto';
	}
  
	function handleAddTask(event) {
	  const newTask = event.detail; // Assurez-vous que les données sont bien émises par le modal
	  tasks = [...tasks, newTask];
	  localStorage.setItem('tasks', JSON.stringify(tasks));
	}
  
	function loadTasks() {
	  const savedTasks = localStorage.getItem('tasks');
	  if (savedTasks) {
		tasks = JSON.parse(savedTasks);
	  }
	}
  
	function toggleCompletion(task) {
	  if (completedTasks.some(t => t.name === task.name && t.date === task.date)) {
		completedTasks = completedTasks.filter(t => !(t.name === task.name && t.date === task.date));
		tasks = [...tasks, task];
	  } else {
		completedTasks = [...completedTasks, task];
		tasks = tasks.filter(t => !(t.name === task.name && t.date === task.date));
	  }
	  localStorage.setItem('tasks', JSON.stringify(tasks));
	  localStorage.setItem('completedTasks', JSON.stringify(completedTasks));
	}
  
	function getDaysRemaining(date) {
	  return moment(date).diff(moment(), 'days');
	}
  
	onMount(() => {
	  loadTasks();
	  const savedCompletedTasks = localStorage.getItem('completedTasks');
	  if (savedCompletedTasks) {
		completedTasks = JSON.parse(savedCompletedTasks);
	  }
	});
  </script>
  
  {#if modalIsActived}
	<Modal {actived} on:add={handleAddTask} />
  {/if}
  
  <Header />
  
  <section>
	<ul class="to-do-list-container">
	  {#each tasks as task}
		<li class="to-do-list-item">
		  <div class="to-do-list-container-item">
			<img src={task.category.icon} alt={task.category.name} />
			<div class="to-do-list-text">
			  <h4>{task.name}</h4>
			  <span>{getDaysRemaining(task.date)} jours restant</span>
			  <label>
				<input
				  type="checkbox"
				  checked={completedTasks.some(t => t.name === task.name && t.date === task.date)}
				  on:change={() => toggleCompletion(task)}
				/>
				{#if completedTasks.some(t => t.name === task.name && t.date === task.date)}
				{/if}
			  </label>
			</div>
		  </div>
		</li>
	  {/each}
	</ul>
	<button on:click={actived}>Add New Task</button>
</section>

	<section>
		<h2>Completed Tasks</h2>
		<ul class="to-do-list-container">
		  {#each completedTasks as task}
			<li class="to-do-list-item">
			  <div class="to-do-list-container-item">
				<img src={task.category.icon} alt={task.category.name} />
				<div class="to-do-list-text">
				  <h4>{task.name}</h4>
				  <span>{getDaysRemaining(task.date)} jours restant</span>
				  <label>
					<input
					  type="checkbox"
					  checked={true}
					  on:change={() => toggleCompletion(task)}
					/>
				  </label>
				</div>
			  </div>
			</li>
		  {/each}
		</ul>
	  </section>



	
  
	

<style>
	button {
		all: unset;
		cursor: pointer;
		padding: 10px;
		text-align: center;
		background-color: #4A3780;
		width: 358px;
		height: 56px;
		gap: 10px;
		border-radius: 50px;
		color: white;
	}
	
	img {
		width: 48px;
		height: 48px;
		border-radius: 50%;
		display: flex;
	}
	
	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 20px;
		gap: 24px;
	}
	
	.to-do-list-container {
		width: 25%;
		list-style: none;
		border-bottom: 1px solid #4A3780;
		display: flex;
		flex-direction: column;
		background-color: white;
		border-radius: 10px;
	}
	
	.to-do-list-item {
		display: flex;
		align-items: center; 
		gap: 12px; 
	}
	
	.to-do-list-container-item {
		display: flex;
		align-items: center; 	
		gap: 12px; 
	}
	
	.to-do-list-text {
		display: flex;
		flex-direction: column;
		margin-bottom: 10%;
	}

	.to-do-list-text span {
		margin-top: auto;
	}
</style>
