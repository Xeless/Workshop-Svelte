<script>
	import moment from "moment";
	let now = moment();
	
	export let taskName = "";
	export let taskDescription = "";
	export let taskDate = "";
	export let errorFound = false; 
	
	console.log(`${now} date du form : ${taskDate}`);
	
	export let errors = {taskName: "", taskDescription: "", taskDate: ""};
	
	function validate(name, description, date) {
		errorFound = false;
		let tempErrors = {taskName: "", taskDescription: "", taskDate: ""};
	
		// Les messages d'erreur pour taskName
		if (name === "") {
			tempErrors.taskName = "Ce champ est vide !";
		} else if (name.length < 3) {
			tempErrors.taskName = `Il manque ${3 - name.length} charactères dans votre task name`;
		} else if (name.length > 10) {
			tempErrors.taskName = `Il y a ${name.length - 10} charactères en trop dans votre task name`;
		}
	
		// Les messages d'erreur pour taskDescription
		if (description === "") {
			tempErrors.taskDescription = "Ce champ est vide !";
		} else if (description.length < 15) {
			tempErrors.taskDescription = `Il manque ${15 - description.length} charactères dans votre description`;
		} else if (description.length > 255) {
			tempErrors.taskDescription = `Il y a ${description.length -255} charactères en trop dans votre description`;
		}
	
		// Les messages d'erreur pour taskDate
		if (date === "") {
			tempErrors.taskDate = "Ce champ est vide !";
		}
	
		
		errors = tempErrors;

		if (errors.taskName || errors.taskDescription || errors.taskDate) {
			errorFound = true;
		}
	}

	function handleKeyup() {
		if (errorFound) {
			validate(taskName, taskDescription, taskDate);
		}
		
	}
	
	function addTask(event) {
		event.preventDefault();
	
		validate(taskName, taskDescription, taskDate);
	
		if (!errorFound) {
			console.log("Ajouter la tâche au local storage");
		} else {
			console.log("Des erreurs ont été trouvées, la tâche n'a pas été ajoutée");
		}
	}
	</script>
	
	<section>
		<div class="modal-header">
			<button>X</button>
			<div class="title-header">
				<h2>Add new task</h2>
			</div>
		</div>
		<form on:submit={addTask}>
			<label for="taskName">Nom de la tâche :</label>
			<br>
			<input type="text" name="taskName" id="taskName" bind:value={taskName} on:keyup={handleKeyup}>
			{#if errors.taskName}
			<p class="error">{errors.taskName}</p>
			{/if}

			<label for="taskDate">Date de la tâche :</label>
			<br>
			<input type="date" name="taskDate" id="taskDate" bind:value={taskDate}  on:keyup={handleKeyup}>
			{#if errors.taskDate}
			<p class="error">{errors.taskDate}</p>
			{/if} 
	
			<label for="taskDescription">Description de la tâche :</label>
			<textarea name="description" id="taskDescription" cols="30" rows="10" bind:value={taskDescription} on:keyup={handleKeyup}></textarea>
			{#if errors.taskDescription}
			<p class="error">{errors.taskDescription}</p>
			{/if}
	
			
	
			<input type="submit" value="Ajouter la tâche">
		</form>
	
		<h1>{taskName}</h1>
	</section>
	
	<style scoped lang="scss">
		section {
			width: 100%;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			flex: 0.6;

			.modal-header {
				width: 100%;
				background-color: #4A3780;
				border-top-left-radius: 15px;
				border-top-right-radius: 15px;
				color: white;
				padding: 10px;
				display: flex;
				flex-direction: row;
				justify-content: space-between;

				button {
					border-radius: 100%;
					border: none;
					width: 50px;
					height: 50px;
				}

				.title-header {
					width: 90%;
					text-align: center;
				}
			}
	
			form {
				position: relative;
				width: 100%;
				height: 80vh;
				display: flex;
				flex-direction: column;
				justify-content: space-around;
				align-items: center;
				gap: 5px;
				padding-top: 10px;

				input {
					width: 90%;
					height: 50px;
					padding-left: 5px;
					border-radius: 5px;
				}

				textarea {
					width: 90%;
				}
				
				.error {
					color: red;
					font-size: 0.9em;
				}
			}
		}
	
		h1 {
			width: 100%;
		}
	</style>
	