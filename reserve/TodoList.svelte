<script>
	import { onDestroy, onMount } from 'svelte';
	import { Router, Route, Link } from "svelte-navigator";
	import { list } from '../stores.js'
	import AddTask  from './AddTask.svelte'
	// const list = [
    // 	{
      // 		id: 1,
      // 		status: 'To do',
      // 		content: 'Play game',
      // 		date: '10/11/2021'
      // 	},
      // 	{
        // 		id: 2,
        // 		status: 'Doing',
        // 		content: 'Clean task',
        // 		date: '30/10/2021'
        // 	},
        // 	{
          // 		id: 3,
          // 		status: 'Done',
          // 		content: 'Buy English book',
          // 		date: '24/9/2021'
          // 	}
          // ]
  export let name;
  let toggleShowList = true;
	let toggleShowAdd = false;
	let list_data = [];
	// let newStatus = "To do";
	// let newContent = "";
	// let newDate = "";
	// let today = new Date().toLocaleDateString()

	onMount(() => {
		console.log('onMount already.');	
		setListData()
	})

	onDestroy(() => {
		console.log('onDestroy App');
	})
	
	const setListData = () => {
		list.subscribe(value => {
			list_data = value
			// console.log(list_data);
		})
	}

	const changeStatusToTodo = (id) => {
		return list_data = list_data.map((item, index) => {
			return id == item.id ? {...item, status: 'To do'} : item
		})
	}

	const changeStatusToDoing = (id) => {
		return list_data = list_data.map((item, index) => {
			return id == item.id ? {...item, status: 'Doing'} : item
		})
	}

	const changeStatusToDone = (id) => {
		return list_data = list_data.map((item, index) => {
			return id == item.id ? {...item, status: 'Done'} : item
		})
	}

	const editTask = (id) => {
		const dateToEdit = prompt('Enter Date')
		const textToEdit = prompt('Enter content')

		return list_data = list_data.map((item, index) => {
			return id == item.id ? {...item, content: textToEdit || item.content, date: dateToEdit || item.date} : item
		})
	}

	const deleteTask = (id) => {
		return list_data = list_data.filter((item, index) => {
			return item.id !== id
		})
	}
  
	export const backToAllList = () => {
		newStatus = "To do";
		newContent = "";
		newDate = "";
		toggleShowAdd = false;
		toggleShowList = true;
	}

	export const addTask = (event) => {
		// list_data = [...list_data, {status: newStatus, content: newContent, date: newDate || today, id: list_data[list_data.length-1].id + 1}]
		// newStatus = "To do";
		// newContent = "";
		// newDate = "";
		const { detail } = event;
		list_data = [...list_data, {...detail, id: list_data[list_data.length-1].id + 1}]
		// console.log('list_data', list_data);
	}
</script>

<Router>
	<main>
		<Route path="/">
		<div class="head">
			<h1>Todo List of {name}!</h1>

				<div>
					<Link to="add-task">Add Task</Link>
				</div>
		</div>
		<div class="all">
			<div class="todo-column">
				<div class="topic">
					<p>Todo</p>
				</div>
				{#each list_data as list}
					{#if list.status == 'To do'}
						<div class="content-box">
							<p>{list.date}</p>
							<p>{list.content}</p>
							<div class="action-button">
								<div>
									<button on:click={() => editTask(list.id)}>Edit</button>
									<button on:click={() => deleteTask(list.id)}>Delete</button>
								</div>
								<div>
									<button on:click={() => changeStatusToDone(list.id)}>Done</button>
									<button on:click={() => changeStatusToDoing(list.id)}>Doing</button>
								</div>
							</div>
						</div>
					{/if }
				{/each}
			</div>
			<div class="doing-column">
				<div class="topic">
					<p>Doing</p>
				</div>
				{#each list_data as list}
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
									<button on:click={() => changeStatusToTodo(list.id)}>Todo</button>
									<button on:click={() => changeStatusToDone(list.id)}>Done</button>
								</div>
							</div>
						</div>
					{/if }
				{/each}
			</div>
			<div class="done-column">
				<div class="topic">
					<p>Done</p>
				</div>
				{#each list_data as list}
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
									<button on:click={() => changeStatusToDoing(list.id)}>Doing</button>
									<button on:click={() => changeStatusToTodo(list.id)}>Todo</button>
								</div>
							</div>
						</div>
					{/if}
				{/each}
			</div>
		</div>
		<!-- <input type="text" placeholder="status" bind:value={newStatus}>  -->
			<!-- <div>
				<h3>Add Task</h3>
				<select name="status" bind:value={newStatus}>
					<option value="To do">To do</option>
					<option value="Doing">Doing</option>
					<option value="Done">Done</option>
				</select>
				<input type="date" placeholder="date" bind:value={newDate}>
				<input type="text" placeholder="content" bind:value={newContent}>
				<button on:click={addTask}>Add</button>
				<button on:click={backToAllList}>Back</button>
			</div> -->
		</Route>

		<Route path="/add-task">
			<AddTask on:addTask={addTask} on:backToAllList={backToAllList} />
		</Route>

	</main>
</Router>


<style>
	
	.all {
		width: 100%;
		display: flex;
		justify-content: center;
	}

	.head {
		text-align: center;
	}

	.todo-column {
		width: 30%;
		height: 500px;
		margin: 10px 10px;
		border: 1px solid #222;
	}

	.doing-column {
		width: 30%;
		height: 500px;
		margin: 10px 10px;
		border: 1px solid #222;
	}

	.done-column {
		width: 30%;
		height: 500px;
		margin: 10px 10px;
		border: 1px solid #222;
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