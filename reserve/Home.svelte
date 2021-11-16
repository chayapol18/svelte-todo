<script>
  import { beforeUpdate } from 'svelte'
  import AddNewTask from './AddNewTask.svelte'
	export let list = [];
  export let newTaskContent = ''
  export let newTaskDate = ''
  let today = new Date().toLocaleDateString('en-GB')
  export let status = 'Todo'
  export let taskType = [
    {id: 1, name: 'Todo'},
    {id: 2, name: 'Doing'},
    {id: 3, name: 'Done'},
  ]

  $: console.log('status', status);
  $: console.log('list.length', list[list.length - 1].id + 1);
  $: console.log('newTaskContent: ', newTaskContent)

  beforeUpdate(() => {
    list.sort(function (a, b) {
    return b.id - a.id;
    })
    console.log('list: ', list)
  })

	const changeStatus = (id, status) => {
		return list = list.map((item) => {
			return id == item.id ? {...item, status} : item
		})
	}

  const AddTask = (detail) => {
    list = [...list, {...detail, id: list[0].id + 1}]
    console.log('32', list);
    newTaskContent = ''
    newTaskDate = ''
    status = 'Todo'
  }

	const editTask = (id) => {
		const dateToEdit = prompt('Enter Date')
		const textToEdit = prompt('Enter content')

		return list = list.map((item, index) => {
			return id == item.id ? {...item, content: textToEdit || item.content, date: dateToEdit || item.date} : item
		})
	}

	const deleteTask = (id) => {
		return list = list.filter((item, index) => {
			return item.id !== id
		})
	}
</script>
<div>
  <div class="head">
    <h1>Svelte Todo List</h1>
  </div>

  <!-- add new task -->
  <div>
    <input type="date" bind:value={newTaskDate}>
    <input type="text" placeholder="What's your task ?" bind:value={newTaskContent}>
    <select bind:value={status}>
      {#each taskType as type}
        <option value={type.name}>
          {type.name}
        </option>
      {/each}
    </select>

    <button on:click={() => AddTask({status: status, date: newTaskDate || today, content: newTaskContent})}>Add Task</button>
  </div>

  <AddNewTask on:AddTask={e => AddTask(e.detail)} />

  <!-- all tasks list -->
  <div class="all">
    <div class="list-column">
      <div class="topic">
        <p>Todo</p>
      </div>
      {#each list as list}
        {#if list.status == 'Todo'}
          <div class="content-box">
            <p>{list.date}</p>
            <p>{list.content}</p>
            <div class="action-button">
              <div>
                <button on:click={() => editTask(list.id)}>Edit</button>
                <button on:click={() => deleteTask(list.id)}>Delete</button>
              </div>
              <div>
                <button on:click={() => changeStatus(list.id, 'Done')}>Done</button>
                <button on:click={() => changeStatus(list.id, 'Doing')}>Doing</button>
              </div>
            </div>
          </div>
        {/if }
      {:else}
      <div class="text-center">
        <p>No Todo Task here</p>
      </div>
      {/each}
    </div>
    <div class="list-column">
      <div class="topic">
        <p>Doing</p>
      </div>
      {#each list as list}
        {#if list.status == 'Doing'}
          <div class="content-box">
            <p>{list.date}</p>
            <p>{list.content}</p>
            <div class="action-button">
              <div>
                <button on:click={() => editTask(list.id)}>Edit</button>
                <button on:click={() => deleteTask(list.id)}>Delete</button>
              </div>
              <div>
                <button on:click={() => changeStatus(list.id, 'Todo')}>Todo</button>
                <button on:click={() => changeStatus(list.id, 'Done')}>Done</button>
              </div>
            </div>
          </div>
        {/if }
      {:else}
      <div class="text-center">
        <p>No Doing Task here</p>
      </div>
      {/each}
    </div>
    <div class="list-column">
      <div class="topic">
        <p>Done</p>
      </div>
      {#each list as list}
        {#if list.status == 'Done'}
          <div class="content-box">
            <p>{list.date}</p>
            <p>{list.content}</p>
            <div class="action-button">
              <div>
                <button on:click={() => editTask(list.id)}>Edit</button>
                <button on:click={() => deleteTask(list.id)}>Delete</button>
              </div>
              <div>
                <button on:click={() => changeStatus(list.id, 'Doing')}>Doing</button>
                <button on:click={() => changeStatus(list.id, 'Todo')}>Todo</button>
              </div>
            </div>
          </div>
        {/if}
      {:else}
      <div class="text-center">
        <p>No Done Task here</p>
      </div>
      {/each}
    </div>
  </div>

</div>

<style>
  .text-center {
    text-align: center
  }
	
	.all {
		width: 100%;
		display: flex;
		justify-content: center;
	}

	.head {
		text-align: center;
	}

	.list-column {
		width: 30%;
		height: 500px;
		margin: 10px 10px;
		border: 1px solid #222;
    overflow: auto;
	}
	.topic {
		width: 100%;
		border-bottom: 1px solid black;
		text-align: center;
	}

	.content-box {
		width: 90%;
		border: 1px solid black;
		margin: 5px auto;
		padding: 5px;
	}

	.action-button {
		display: flex;
		justify-content: space-between;
	}
</style>