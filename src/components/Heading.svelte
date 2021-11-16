<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher()

  export let isOpen; 

  const openAddTask = (status) => {
    dispatch('openAddTask', status)
  }

</script>


<div class="head text-center shadow-md text-black">
  <h1 class="text-4xl font-bold">Svelte Todo List</h1>
  <div class="absolute top-0 right-0 mt-4 mr-8">
    {#if isOpen == false}
    <button class="px-2 py-1 rounded-md bg-transparent border-0 font-semibold text-red-500 text-xl" on:click={() => openAddTask(true)}>
      Add Task
    </button>
    {:else}
    <button class="px-2 py-1 rounded-md bg-transparent border-0 font-semibold text-red-500 text-xl" on:click={() => openAddTask(false)}>
      Close Add Task
    </button>
    {/if}
  </div>
</div>

<style>
  .head {
    margin: 0 auto;
    padding: 25px;
    /* background: linear-gradient(-70deg, #fa7c30 30%, rgba(0, 0, 0, 0) 30%), url('planets.jpg'); */
    /* background: linear-gradient(110deg, #304352 65%, #d7d2cc 60%); */
    background: linear-gradient(45deg, #70e1f5, #ffd194);
  }
</style>