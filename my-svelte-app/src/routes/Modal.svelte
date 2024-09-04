<script>
	// Imports
	import moment from "moment";

	// Variables
	let now = moment().subtract(10, 'days').calendar();
	export let taskName = "";
	export let taskDescription = "";
	export let taskDate = "";
	export let errorFound = false; 
	export let errors = {taskName: "", taskDescription: "", taskDate: ""};

	// Props
	export let actived;

	function close() {
		if (typeof actived === 'function') {
			actived(); 
		}
	}
	
	
	
	// functions
	function validate(name, description, date) {
		errorFound = false;
		let tempErrors = {taskName: "", taskDescription: "", taskDate: ""};

		// Error messages for taskName
		if (name !== null) {
			if (name === "") {
			tempErrors.taskName = "This field is empty!";
			} else if (name.length < 3) {
				tempErrors.taskName = `You are missing ${3 - name.length} characters in your task name`;
			} else if (name.length > 25) {
				tempErrors.taskName = `There are ${name.length - 25} extra characters in your task name`;
			}
		}
		

		// Error messages for taskDescription
		if (description !== null) {
			if (description === "") {
			tempErrors.taskDescription = "This field is empty!";
			} else if (description.length < 15) {
				tempErrors.taskDescription = `You are missing ${15 - description.length} characters in your description`;
			} else if (description.length > 255) {
				tempErrors.taskDescription = `There are ${description.length - 255} extra characters in your description`;
			}
		}
		

		// Error messages for taskDate
		if ( date !== null) {
			if (date === "") {
			tempErrors.taskDate = "This field is empty!";
			} else if (moment(date).isBefore(moment(), 'day')) {
				tempErrors.taskDate = "The date cannot be earlier than today.";
			}
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
		console.log(`${now} date du form : ${taskDate}`);
		if (!errorFound) {
			console.log("Ajouter la tâche au local storage");
		} else {
			console.log("Des erreurs ont été trouvées, la tâche n'a pas été ajoutée");
		}
	}

	</script>
	
	<section>
		<div class="modal-header">
			<button on:click={close}>X</button>
			<div class="title-header">
				<h2>Add new task</h2>
			</div>
		</div>
		<form on:submit={addTask}>
			<label for="taskName">Nom de la tâche :</label>
			<br>
			<input type="text" name="taskName" id="taskName" bind:value={taskName} on:keyup={validate(taskName, null,  null)}>
			{#if errors.taskName}
			<p class="error">{errors.taskName}</p>
			{/if}

			<label for="taskDate">Date de la tâche :</label>
			<br>
			<input type="date" name="taskDate" id="taskDate" bind:value={taskDate}  on:change={validate(null, null,  taskDate)}>
			{#if errors.taskDate}
			<p class="error">{errors.taskDate}</p>
			{/if} 
	
			<label for="taskDescription">Description de la tâche :</label>
			<textarea name="description" id="taskDescription" cols="30" rows="10" bind:value={taskDescription} on:keyup={validate(null, taskDescription, null )}></textarea>
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
					cursor: pointer;
				}

				.title-header {
					width: 55%;
					text-align: left;
				}
			}
	
			form {
				position: relative;
				width: 100%;
				height: 85vh;
				display: flex;
				flex-direction: column;
				justify-content: space-around;
				align-items: center;
				gap: 5px;
				padding-top: 10px;

				label {
					font-weight: bold;
				}
				input {
					width: 90%;
					min-height: 50px;
					padding-left: 5px;
					border-radius: 5px;
				}

				textarea {
					width: 90%;
					min-height: 150px;
				}
				
				.error {
					background-color: tomato;
					padding: 5px 15px;
					border-radius: 5px;
					color: white;
					font-size: 0.9em;
					font-weight: bold;
				}
			}
		}
	
		h1 {
			width: 100%;
		}
	</style>
	