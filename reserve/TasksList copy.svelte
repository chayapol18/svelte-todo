<script>
  import { createEventDispatcher } from 'svelte';
  import Task from './EachTask.svelte'
  const dispatch = createEventDispatcher()
  export let list = [];

  const editTask = (detail) => {
    dispatch('editTask', detail)
  }

  const deleteTask = (id) => {
    dispatch('deleteTask', id)
  }
  
  const deleteAllTasks = () => {
    let checked = confirm(`You want to delete all tasks ?`)
    if (checked == true) {
      dispatch('deleteAllTasks')
    }
  }

  const changeStatus =({id, status}) => {
    dispatch('changeStatus', {id, status})
  }

  //tailwind class
  const listColumn = "shadow-md border-2 rounded-md"
  const topicText = "border-2 py-2.5 w-full text-center text-2xl font-semibold"

  let status = ''
  const filterTasks = (status, list) =>
    status === 'Todo' ? list.filter(t => t.status == 'Todo') :
    status === 'Doing' ? list.filter(t => t.status == 'Doing') :
    status === 'Done' ? list.filter(t => t.status == 'Done') :
    ''
</script>

<!-- <div class="mx-auto mt-4 text-center underline">
  <h2>Your Tasks</h2>
</div> -->

<div class="all mt-4">
  <div class="list-column border-blue-800 bg-blue-50 {listColumn}">
    <div class="{topicText}  border-blue-800 bg-blue-100">
      <p class="mb-0 flex items-center justify-center">
        Todo 
        <span class="text-base ml-2">
          ({list.filter(t => t.status == 'Todo').length} {list.filter(t => t.status == 'Todo').length > 1 ? 'tasks' : 'task'})
        </span> 
      </p>
    </div>
    {#each filterTasks('Todo', list) as task (task.id)}
      <Task {task} 
        on:editTask={e => editTask(e.detail)}
        on:deleteTask={e => deleteTask(e.detail)}
        on:changeStatus={e => changeStatus(e.detail)}
      />
    {:else}
      <div class="text-center mt-4">
        <p>No Todo Task here</p>
      </div>
    {/each}
  </div>
  <div class="list-column border-red-800 bg-red-50 {listColumn} ">
    <div class="{topicText} bg-red-100 border-red-800">
      <!-- <p class="mb-0">Doing</p> -->
      <p class="mb-0 flex items-center justify-center">
        Doing 
        <span class="text-base ml-2">
          ({list.filter(t => t.status == 'Doing').length} {list.filter(t => t.status == 'Doing').length > 1 ? 'tasks' : 'task'})
        </span> 
      </p>
    </div>
    {#each filterTasks('Doing', list) as task (task.id)}
      <Task {task} 
        on:editTask={e => editTask(e.detail)}
        on:deleteTask={e => deleteTask(e.detail)}
        on:changeStatus={e => changeStatus(e.detail)}
      />
    {:else}
      <div class="text-center mt-4">
        <p>No Doing Task here</p>
      </div>
    {/each}
  </div>
  <div class="list-column border-green-800 bg-green-50 {listColumn}">
    <div class="{topicText} border-green-800 bg-green-100">
      <!-- <p class="mb-0">Done</p> -->
      <p class="mb-0 flex items-center justify-center">
        Done 
        <span class="text-base ml-2">
          ({list.filter(t => t.status == 'Done').length} {list.filter(t => t.status == 'Done').length > 1 ? 'tasks' : 'task'})
        </span> 
      </p>
    </div>
    {#each filterTasks('Done', list) as task (task.id)}
      <Task {task} 
        on:editTask={e => editTask(e.detail)}
        on:deleteTask={e => deleteTask(e.detail)}
        on:changeStatus={e => changeStatus(e.detail)}
      />
    {:else}
      <div class="text-center mt-4">
        <p>No Done Task here</p>
      </div>
    {/each}
  </div>
</div>

<div class="bottom-button flex items-center w-full">
  <button on:click={deleteAllTasks} class="w-1/4 py-2 m-auto bg-red-500 text-white rounded-md shadow-md border-0">Delete All Tasks</button>
</div>


<style>

	.all {
		width: 100%;
		display: flex;
		justify-content: center;
    padding-bottom: 10px;
	}

	.list-column {
		/* width: 30%; */
		height: 520px;
		margin: 10px 10px;
    overflow: auto;
	}

  .each-column {
    width: 30%;
  }

	.content-box {
		width: 90%;
		margin: 12px auto;
		padding: 12px;
	}

	.action-button {
		display: flex;
		justify-content: space-between;
	}

  .bottom-button {
    padding-bottom: 20px;
  }
</style>