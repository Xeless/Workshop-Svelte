<script>
	import Header from './Header.svelte';
  
	let tasks = [
	  {

		nom: "WorkShop",
		date: "12 jours restant"
	  },
	
	];
  </script>
  

  <Header />

  
  <section>
	<ul class="to-do-list-container">	
	  {#each tasks as task}
		<li class="to-do-list-item">
			<div class="to-do-list-container-item">
				<img src="/Category-Goal.png" alt="Goal">
				<div class="to-do-list-text">
					<h4>{task.nom}</h4>
					<span>{task.date}</span>
				</div>
			</div>
		</li>
	  {/each}
	</ul>
  
	<button>Add New Task</button>
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
	