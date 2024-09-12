<script>
    export let actived;

    // Fonction pour fermer/modifier l'état de modal
    function close() {
        if (typeof actived === 'function') {
            actived(); 
        }
    }

    let tasks = [];

    // Charger les tâches depuis localStorage
    if (typeof window !== 'undefined') {
        tasks = JSON.parse(localStorage.getItem('task')) || [];
        console.log(tasks);   
    }

    function daysBetweenDates(date) {
        const d1 = new Date();
        const d2 = new Date(date);
        const diffTime = d2 - d1;
        const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        return diffDays;
    }

    function markAsCompleted(index) {
        tasks = tasks.map((task, i) => 
            i === index ? { ...task, completed: !task.completed } : task
        );
        localStorage.setItem('task', JSON.stringify(tasks));
    }

    $: completedTasks = tasks.filter(task => task.completed);
</script>

<section class="todo">
    <div class="header">
        <h1>TODO</h1>
    </div>
    <div class="tasks">
        {#each tasks as task, index}
        {#if !task.completed}
        <article class="task">
            <div class="content">
                <h3><img src={task.category.icon} alt={task.name}> { task.name }</h3>
                <input type="checkbox" checked={task.completed} on:change={() => markAsCompleted(index)}>
            </div>
            
            <div class="time">
                {#if daysBetweenDates(task.date) < 0}
                    <p style="color: red">Expired</p>
                {/if}

                {#if daysBetweenDates(task.date) >= 0 && daysBetweenDates(task.date) < 2}
                    <p style="color: tomato">{daysBetweenDates(task.date)} day remaining</p>
                {/if}

                {#if daysBetweenDates(task.date) > 1 && daysBetweenDates(task.date) < 4}
                    <p style="color: orange">{daysBetweenDates(task.date)} days remaining</p>
                {/if}

                {#if daysBetweenDates(task.date) >= 4}
                    <p style="color: lightseagreen">{daysBetweenDates(task.date)} days remaining</p>
                {/if}
            </div>
        </article>
        {/if}
    {/each}
    </div>
    <div class="tasks-not-completed">
        <h2>Task Completed</h2>
        {#if completedTasks.length > 0}
            {#each completedTasks as task, index}
            <article class="task">
                <div class="content">
                    <h3 class="completed">
                        <img src={task.category.icon} alt={task.name}> { task.name }
                    </h3>
                    <input type="checkbox" checked={task.completed} on:change={() => markAsCompleted(tasks.findIndex(t => t === task))}>
                </div>
                <div class="time">
                    <p style="color: lightseagreen">Completed</p>
                </div>
            </article>
            {/each}
        {:else}
            <p>No completed tasks yet.</p>
        {/if}
    </div>

    <button class="add-task" on:click={close}>Add task</button>
</section>

<style lang="scss" scoped>
    section {
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    :where(.tasks-not-completed, .tasks){
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: start;
        align-items: center;
        max-height: 250px;
        min-height: 250px;
        overflow: scroll;
    }
    .tasks-not-completed {
        h2 {
            width: 300px;
        }
    }
    .task {
        padding: 15px;
        width: 300px;
        background-color: rgb(227, 227, 227);
        margin-bottom: 20px;
        border-radius: 10px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        min-height: 80px;
        max-height: 80px;
        .content {
            max-height: 50%;
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
            img {
                position: relative;
                top: 5px;
            }
        }
        .time {
            position: relative;
            bottom: 5px;
            width: 100%;
            text-align: end;
        }
        .completed {
            text-decoration-line: line-through;
        }
    }

    .add-task {
        width: 300px;
        height: 50px;
        border-radius: 10px;
        background-color: #4A3780;
        color: white;
        cursor: pointer;
        transition: 0.3s ease-out;
        &:hover {
            background-color: #654da9;
        }
    }
</style>
