<script>
  import { createEventDispatcher, onMount, onDestroy } from 'svelte';
  const dispatch = createEventDispatcher()
  let status = 'Todo'
  let content = ''
  let date = ''
  let today = new Date().toLocaleDateString('en-GB')
  let taskType = [
    {id: 1, name: 'Todo'},
    {id: 2, name: 'Doing'},
    {id: 3, name: 'Done'},
  ]
  let ref;
  

  onMount(() => {
    ref.focus()
  })

  const addTask = () => {
    dispatch('addTask', {
      date: date || today, 
      content: content || 'New Task',
      status
    })
    // status = 'Todo',
    // content = '',
    // date = ''
  }

  const onKeyPress = e => {
    if (e.charCode === 13 && content) addTask();
  };

  onDestroy(() => {
    status = 'Todo',
    content = '',
    date = ''
    console.log('Add task component is destroyed');
  })
</script>

<div on:keypress={onKeyPress} class="add-task-box flex flex-col items-center mt-8 mx-auto p-4 border-2 border-yellow-400 bg-white shadow-md rounded-md">
  <div class="flex justify-between w-full">
    <div class="w-2/3">
      <div class="">
        <span>Date : </span>
        <input type="date" bind:value={date} class="w-4/5 h-8 text-left pl-2">
      </div>
  
      <div class="mt-2">
        <span>Task : </span>
        <input type="text" placeholder="fill your task" bind:value={content} class="w-4/5 h-8 text-left pl-2" bind:this={ref}>
      </div>
    </div>

    <div class="flex">
      <span class="mr-2">Status : </span>
      <!-- <select bind:value={status} class="h-8 text-center">
        {#each taskType as type}
          <option value={type.name}>
            {type.name}
          </option>
        {/each}
      </select> -->
      <div class="flex flex-col">
        <div class="flex items-center mr-2">
          <input type="radio" bind:group={status} value="Todo">
          <span class="ml-1">Todo</span>
        </div>
        <div class="flex items-center mr-2">
          <input type="radio" bind:group={status} value="Doing">
          <span class="ml-1">Doing</span>
        </div>
        <div class="flex items-center">
          <input type="radio" bind:group={status} value="Done">
          <span class="ml-1">Done</span>
        </div>
      </div>
    </div>
  </div>
  <div class="mt-4 w-full items-center">
    {#if !content }
      <button disabled class="px-4 py-2 w-full rounded-md border-2 border-gray-500 bg-gray-100 font-semibold shadow-md">Add New Task</button>
    {:else}
      <button on:click={() => addTask()} class="px-4 py-2 w-full rounded-md border-2 border-yellow-400 bg-yellow-100 font-semibold text-yellow-600 shadow-md">Add New Task</button>
    {/if}
  </div>
</div>


<style>
  .add-task-box {
    width: 480px
  }

</style>