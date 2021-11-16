<script>
  import { onDestroy, onMount, createEventDispatcher } from 'svelte';
  import { Router, Route, Link, useNavigate } from "svelte-navigator";
	const navigate = useNavigate();
  const dispatch = createEventDispatcher()
  let status = "Todo";
  let date = "";
  let content = "";
  let today = new Date().toLocaleDateString()

	onMount(() => {
		console.log('onMount AddTask already.');	
	})

	onDestroy(() => {
		console.log('onDestroy AddTask');
	})
	
</script>

<Router>
  <div>
    <h3>Add Task</h3>
    <!-- <input type="text" placeholder="status" bind:value={newStatus}>  -->
    <select name="status" bind:value={status}>
      <option value="Todo">To do</option>
      <option value="Doing">Doing</option>
      <option value="Done">Done</option>
    </select>
    <input type="date" placeholder="date" bind:value={date}>
    <input type="text" placeholder="content" bind:value={content}>
    <button on:click={() => { 
      dispatch('addTask', {status, date: date || today, content});
      navigate('/');
      }
    }>
      Add
    </button>
    <!-- <button on:click={backToAllList}>Back</button> -->
    <Link to="/">Back</Link>
  </div>
</Router>