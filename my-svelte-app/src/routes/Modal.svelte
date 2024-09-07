<script>
	// Imports
	import moment from "moment";
	import Document from "./../lib/images/Document.svg";
	import Basketball from "./../lib/images/Basketball.svg";
	import Calendar from "./../lib/images/Calendar.svg";

	// Variables
	let now = moment().subtract(10, 'days').calendar();
	export let taskName = "";
	export let taskDescription = "";
	export let categorys = [
		{name: "study", icon: Document},
		{name: "sport", icon: Basketball},
		{name: "event", icon: Calendar}
	];
	export let categorySelected = [];
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
		console.log(categorySelected);
		
		validate(taskName, taskDescription, taskDate);
		console.log(`${now} date du form : ${taskDate}`);
		if (!errorFound) {
			console.log("Ajouter la tâche au local storage");
		} else {
			console.log("Des erreurs ont été trouvées, la tâche n'a pas été ajoutée");
		}
	}


	let selectedcategoryFunction = () => {
		categorySelected = [{name: category.name, icon: category.icon}]
	}

	function saveTask(name, date, description, category) {
    const task = { name, date, description, category };
    localStorage.setItem('task', JSON.stringify(task)); 
    
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
    <label for="taskName">Task name :</label>
    <br />
    <input
      type="text"
      name="taskName"
      id="taskName"
      placeholder="Task name"
      bind:value={taskName}
      on:keyup={validate(taskName, null, null)}
    />
    {#if errors.taskName}
      <p class="error">{errors.taskName}</p>
    {/if}
    <label for="category">Category :</label>
    <br />
    <div class="categorys">
      {#each categorys as category}
        <div
          class="category {categorySelected.name === category.name
            ? 'selected'
            : ''}"
          role="button"
          tabindex="0"
          value={category.name}
          on:click={() => (categorySelected = category)}
          on:keydown={(event) =>
            event.key === "Enter" && (categorySelected = category)}
        >
          <img src={category.icon} alt={category.name} />
          <p>{category.name}</p>
        </div>
      {/each}
    </div>
    <label for="taskDate">Task date :</label>
    <br />
    <input
      type="date"
      name="taskDate"
      id="taskDate"
      bind:value={taskDate}
      on:change={validate(null, null, taskDate)}
    />
    {#if errors.taskDate}
      <p class="error">{errors.taskDate}</p>
    {/if}

    <label for="taskDescription">Task description :</label>
    <textarea
      name="description"
      id="taskDescription"
      cols="30"
      rows="10"
      bind:value={taskDescription}
      on:keyup={validate(null, taskDescription, null)}
    ></textarea>
    {#if errors.taskDescription}
      <p class="error">{errors.taskDescription}</p>
    {/if}

    <input type="submit" on:click={() => saveTask(taskName, taskDate, taskDescription, categorySelected)} value="Ajouter la tâche" />
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
      background-image: url("./../lib/images/Header\ \(1\).svg");
      background-size: cover;
      background-color: #4a3780;
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
      height: 95vh;
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      align-items: center;
      gap: 5px;
      padding-top: 10px;

      label {
        font-weight: bold;
        margin-top: 10px;
      }
      input {
        width: 90%;
        min-height: 50px;
        padding-left: 5px;
        border-radius: 5px;
      }
      textarea {
        margin-top: 15px;
        border-radius: 5px;
      }
      input[type="submit"] {
        margin-top: 30px;
        margin-bottom: 30px;
        background-color: #4a3780;
        color: white;
        border-radius: 60px;
      }

      .categorys {
        display: flex;
        flex-direction: row;
        gap: 25px;
        .category {
          padding: 10px;
          width: 70px;
          height: 100px !important;
          border-radius: 15px;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
          align-items: center;
          &:hover {
            background-color: rgba(0, 255, 255, 0.222);
          }
          img {
            width: 80%;
          }
          p {
            font-weight: bold;
          }
        }
        .selected {
          background-color: rgba(0, 255, 187, 0.222);
          border: 2px black solid;
        }
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
