<script>
	import Header from './Header.svelte';
  
	let tasks = [
	  {
		   categorie: "<svg width=\"48\" height=\"48\" viewBox=\"0 0 48 48\" fill=\"none\" xmlns=\"http://www.w3.org/2000/svg\">\,",
		nom: "WorkShop",
		date: "04/09/1998",
		time: "12H"
	  },
	
	];
  </script>
  
  <Header />
  
  <section>
	<ul>
	  {#each tasks as task}
		<li>
		  <h4>{task.categorie}{task.nom}</h4>
		  <p>{task.date},  {task.time}</p>
		</li>
	  {/each}
	</ul>
  
	<button>Add New Task</button>
  </section>
  
<style>
button{
	all: unset; 
        cursor: pointer; 
        padding: 10px; 
		text-align: center; 
		background-color: #4A3780;
		width: 358px;
		height: 56px;
		top: 764px;
		left: 16px;
		gap: 10px;
		border-radius: 50px;
		color: white;
}

li{
	display: inline-block;
}

	section {
		display: flex;
		flex-direction: column;
		align-items: center;
		width: Fixed (358px)px;
		height: Hug (470px)px;
		top: 158px;
		left: 16px;
		padding: 10px;
		gap: 24px;
        opacity: 0px;
	}


</style>
