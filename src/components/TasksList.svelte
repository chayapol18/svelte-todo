<script>
  import { createEventDispatcher } from 'svelte';
  import EachTask from './EachTask.svelte'
  const dispatch = createEventDispatcher()
  export let list = [];

  const editTask = (detail) => {
    dispatch('editTask', detail)
  }

  const copyTask = (detail) => {
    dispatch('copyTask', detail)
  }

  const deleteTask = (detail) => {
    dispatch('deleteTask', detail)
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
  const eachColumn = "shadow-md rounded-b-md border-b-2 border-l-2 border-r-2 "
  const topicText = "border-2 py-2.5 w-full text-center text-2xl font-semibold flex items-center justify-center rounded-t-lg "

  let status = ''
  const filterTasks = (status, list) =>
    status === 'Todo' ? list.filter(task => task.status == 'Todo') :
    status === 'Doing' ? list.filter(task => task.status == 'Doing') :
    status === 'Done' ? list.filter(task => task.status == 'Done') :
    ''
</script>


<div class="all mt-10">
  <div class="list-column">
    <div class="{topicText} topic-text border-blue-800 bg-blue-100 ">
      <p class="mb-0 flex items-center justify-center">
        Todo 
        <span class="text-xl ml-2">
          ({list.filter(task => task.status == 'Todo').length} {list.filter(t => t.status == 'Todo').length == 1 ? 'task' : 'tasks'})
        </span> 
      </p>
    </div>
    <div class="{eachColumn} each-column border-blue-800 bg-blue-50 ">
      {#each filterTasks('Todo', list) as task (task.id)}
        <EachTask {task} 
          on:editTask={e => editTask(e.detail)}
          on:deleteTask={e => deleteTask(e.detail)}
          on:changeStatus={e => changeStatus(e.detail)}
          on:copyTask={e => copyTask(e.detail)}
        />
      {:else}
        <div class="text-center mt-4">
          <p>There are no tasks in Todo status.</p>
        </div>
      {/each}
    </div>
  </div>

  <div class="list-column">
    <div class="{topicText} topic-text bg-red-100 border-red-800 ">
      <p class="mb-0 flex items-center justify-center">
        Doing 
        <span class="text-xl ml-2">
          ({list.filter(task => task.status == 'Doing').length} {list.filter(t => t.status == 'Doing').length == 1 ? 'task' : 'tasks'})
        </span> 
      </p>
    </div>
    <div class="{eachColumn} each-column border-red-800 bg-red-50 ">
      {#each filterTasks('Doing', list) as task (task.id)}
        <EachTask {task} 
          on:editTask={e => editTask(e.detail)}
          on:deleteTask={e => deleteTask(e.detail)}
          on:changeStatus={e => changeStatus(e.detail)}
          on:copyTask={e => copyTask(e.detail)}
        />
      {:else}
        <div class="text-center mt-4">
          <p>There are no tasks in Doing status.</p>
        </div>
      {/each}
    </div>
  </div>

  <div class="list-column">
    <div class="{topicText} topic-text border-green-800 bg-green-50 ">
      <p class="mb-0 flex items-center justify-center">
        Done 
        <span class="text-xl ml-2">
          ({list.filter(task => task.status == 'Done').length} {list.filter(t => t.status == 'Done').length == 1 ? 'task' : 'tasks'})
        </span> 
      </p>
    </div>
    <div class="{eachColumn} each-column border-green-800 bg-green-50 ">
      {#each filterTasks('Done', list) as task (task.id)}
        <EachTask {task} 
          on:editTask={e => editTask(e.detail)}
          on:deleteTask={e => deleteTask(e.detail)}
          on:changeStatus={e => changeStatus(e.detail)}
          on:copyTask={e => copyTask(e.detail)}
        />
      {:else}
        <div class="text-center mt-4">
          <p>There are no tasks in Done status.</p>
        </div>
      {/each}
    </div>
  </div>
</div>

<div class="bottom-button flex items-center w-full mt-6">
  <button on:click={deleteAllTasks} class="w-1/4 py-2.5 m-auto bg-red-500 text-white rounded-md shadow-md border-0">Delete All Tasks</button>
</div>


<style>

	.all {
		width: 100%;
		display: flex;
		justify-content: center;
    padding-bottom: 10px;
	}

	.list-column {
		width: 30%;
		height: 560px;
		margin: auto 10px;
	}

  .each-column {
    height: 500px;
    margin: 0 0 10px;
    overflow: auto;
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
    padding-bottom: 10px;
  }

  .topic-text {
    height: 70px;
  }
</style>