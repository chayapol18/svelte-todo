<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher()
  export let task;
  let editing = false;
  let content = ''
  let date = ''

  const openEditing = (task) => {
    content = task.content
    date = task.date
    console.log('date', date);
    editing = true;
  }

  const cancelEditing = () => {
    editing = false;
  }

  const saveTask = (detail) => {
    dispatch('editTask', detail)
    editing = false;
  }

  const copyTask = (detail) => {
    dispatch('copyTask', detail)
  }

  const deleteTask = (task) => {
    let checked = confirm(`You want to delete task ?`)
    if (checked == true) {
      dispatch('deleteTask', task)
    }
  }

  const changeStatus =(id, status) => {
    dispatch('changeStatus', {id, status})
  }

  const onKeyPress = (e, detail) => {
    if (e.charCode === 13 && content) saveTask(detail);
  };

  //tailwind class
  const iconButtonClass = "py-1.5 px-2 rounded-md shadow-sm border-1 bg-gray-100"
  const buttonClass = "px-2 py-1 rounded-md border-1 border-gray-800 shadow-sm "
  const BgColorBox = task.status == 'Todo' ? "border-blue-500" : task.status == 'Doing' ? 'border-red-500' : 'border-green-500'

</script>

{#if editing}
  <div on:keypress={(e) => onKeyPress(e, {id: task.id, date, content})} class="content-box border-2 rounded-md text-sm bg-gray-50 shadow-sm {BgColorBox}">
    <input type="text" bind:value={date} class="w-full mb-2 h-8 text-left pl-2">
    <input type="text" bind:value={content} class="w-full mb-2 h-8 text-left pl-2">
    <div class="flex justify-around items-center">
      <button on:click={() => saveTask({id: task.id, date, content})} class="{buttonClass}">Save</button>
      <button on:click={cancelEditing} class="{buttonClass}">Cancel</button>
    </div>
  </div>
{:else}
  <div class="content-box border-2 rounded-md text-base bg-gray-50 shadow-sm {BgColorBox}">
    <p>{task.date}</p>
    <p class="content-text break-words">{task.content}</p>
    <div class="flex justify-between items-center">
      <div>
        <button on:click={() => openEditing(task)} class="{iconButtonClass} border-gray-800">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
            <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
          </svg>
          <!-- <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pen" viewBox="0 0 16 16">
            <path d="m13.498.795.149-.149a1.207 1.207 0 1 1 1.707 1.708l-.149.148a1.5 1.5 0 0 1-.059 2.059L4.854 14.854a.5.5 0 0 1-.233.131l-4 1a.5.5 0 0 1-.606-.606l1-4a.5.5 0 0 1 .131-.232l9.642-9.642a.5.5 0 0 0-.642.056L6.854 4.854a.5.5 0 1 1-.708-.708L9.44.854A1.5 1.5 0 0 1 11.5.796a1.5 1.5 0 0 1 1.998-.001zm-.644.766a.5.5 0 0 0-.707 0L1.95 11.756l-.764 3.057 3.057-.764L14.44 3.854a.5.5 0 0 0 0-.708l-1.585-1.585z"/>
          </svg> -->
        </button>
        <button on:click={() => copyTask(task)} class="{iconButtonClass} border-yellow-500 text-yellow-500 ml-1.5">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-files" viewBox="0 0 16 16">
            <path d="M13 0H6a2 2 0 0 0-2 2 2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h7a2 2 0 0 0 2-2 2 2 0 0 0 2-2V2a2 2 0 0 0-2-2zm0 13V4a2 2 0 0 0-2-2H5a1 1 0 0 1 1-1h7a1 1 0 0 1 1 1v10a1 1 0 0 1-1 1zM3 4a1 1 0 0 1 1-1h7a1 1 0 0 1 1 1v10a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V4z"/>
          </svg>
        </button>
        <button on:click={() => deleteTask(task)} class="{iconButtonClass} border-red-500 text-red-500 ml-1.5">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
            <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
            <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
          </svg>
        </button>
      </div>
      {#if task.status == 'Todo'}
        <div>
          <button on:click={() => changeStatus(task.id, 'Done')} class="{buttonClass} mr-1.5 bg-green-100">Done</button>
          <button on:click={() => changeStatus(task.id, 'Doing')} class="{buttonClass} bg-red-100">Doing</button>
        </div>
      {:else if task.status == 'Doing'}
        <div>
          <button on:click={() => changeStatus(task.id, 'Todo')} class="{buttonClass} mr-1.5 bg-blue-100">Todo</button>
          <button on:click={() => changeStatus(task.id, 'Done')} class="{buttonClass} bg-green-100">Done</button>
        </div>
      {:else if task.status == 'Done'}
        <div>
          <button on:click={() => changeStatus(task.id, 'Doing')} class="{buttonClass} mr-1.5 bg-red-100">Doing</button>
          <button on:click={() => changeStatus(task.id, 'Todo')} class="{buttonClass} bg-blue-100">Todo</button>
        </div>
      {/if}
    </div>
  </div>
{/if}

<style>
  .content-box {
		width: 90%;
		margin: 12px auto;
		padding: 12px;
	}

  .action-button {
		display: flex;
		justify-content: space-between;
	}

  .content-text {
    margin-top: 10px;
    margin-bottom: 16px;
  }
</style>