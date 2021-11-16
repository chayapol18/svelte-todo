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

  const changeStatus =({id, status}) => {
    dispatch('changeStatus', {id, status})
  }

  //tailwind class
  const listColumn = "bg-white shadow-md border-2 rounded-md"
  const topicText = "border-b-2 py-2 w-full text-center text-2xl font-semibold"
  const iconButtonClass = "p-1 rounded-md shadow-sm"
  const buttonClass = "px-2 py-1 rounded-md "
  const BgColorBox = task.status == 'Todo' ? "border-blue-500" : task.status == 'Doing' ? 'border-red-500' : 'border-green-500'


  let status = ''
  const filterTasks = (status, list) =>
    status === 'Todo' ? list.filter(t => t.status == 'Todo') :
    status === 'Doing' ? list.filter(t => t.status == 'Doing') :
    status === 'Done' ? list.filter(t => t.status == 'Done') :
    ''
</script>

<div class="mx-auto my-4 text-center underline">
  <h2>Your Tasks</h2>
</div>

<div class="all">
  <div class="list-column  border-blue-800 {listColumn}">
    <div class="{topicText}  border-blue-800 bg-blue-100">
      <p class="mb-0">Todo</p>
    </div>
    {#each list as task}
       {#if task.status == 'Todo'}
        <div class="content-box border-2 rounded-md text-sm border-blue-500 bg-gray-50 shadow-sm">
          <p>{task.date}</p>
          <p>{task.content}</p>
          <div class="action-button items-center">
            <div>
              <button on:click={() => editTask(task.id)} class="{iconButtonClass}">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
                  <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
                </svg>
              </button>
              <button on:click={() => deleteTask(task.id)} class="{iconButtonClass}">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
                  <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                  <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                </svg>
              </button>
            </div>
            <div>
              <button on:click={() => changeStatus(task.id, 'Done')} class="{buttonClass}">Done</button>
              <button on:click={() => changeStatus(task.id, 'Doing')} class="{buttonClass}">Doing</button>
            </div>
          </div>
        </div>
      {/if }
    {:else}
      <div class="text-center mt-4">
        <p>No Todo Task here</p>
      </div>
    {/each}
  </div>
  <div class="list-column border-red-800 {listColumn} ">
    <div class="{topicText} bg-red-100 border-red-800">
      <p class="mb-0">Doing</p>
    </div>
    {#each list as task}
      {#if task.status == 'Doing'} 
        <div class="content-box border-2 rounded-md text-sm border-red-500 bg-gray-50 shadow-sm">
          <p>{task.date}</p>
          <p>{task.content}</p>
          <div class="action-button items-center">
            <div>
              <button on:click={() => editTask(task.id)} class="{iconButtonClass}">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
                  <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
                </svg>
              </button>
              <button on:click={() => deleteTask(task.id)} class="{iconButtonClass}">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
                  <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                  <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                </svg>
              </button>
            </div>
            <div>
              <button on:click={() => changeStatus(task.id, 'Todo')} class="{buttonClass}">Todo</button>
              <button on:click={() => changeStatus(task.id, 'Done')} class="{buttonClass}">Done</button>
            </div>
          </div>
        </div> 
      {/if }
    {:else}
      <div class="text-center mt-4">
        <p>No Doing Task here</p>
      </div>
    {/each}
  </div>
  <div class="list-column border-green-800 {listColumn}">
    <div class="{topicText} border-green-800 bg-green-100">
      <p class="mb-0">Done</p>
    </div>
    {#each list as task}
      {#if task.status == 'Done'}
        <div class="content-box border-2 rounded-md text-sm border-green-500 bg-gray-50 shadow-sm">
          <p>{task.date}</p>
          <p>{task.content}</p>
          <div class="action-button items-center">
            <div>
              <button on:click={() => editTask(task.id)} class="{iconButtonClass}">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
                  <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
                </svg>
              </button>
              <button on:click={() => deleteTask(task.id)} class="{iconButtonClass}">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
                  <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                  <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                </svg>
              </button>
            </div>
            <div>
              <button on:click={() => changeStatus(task.id, 'Doing')} class="{buttonClass}">Doing</button>
              <button on:click={() => changeStatus(task.id, 'Todo')} class="{buttonClass}">Todo</button>
            </div>
          </div>
        </div>
      {/if}
    {:else}
      <div class="text-center mt-4">
        <p>No Done Task here</p>
      </div>
    {/each}
  </div>
</div>

<style>

	.all {
		width: 100%;
		display: flex;
		justify-content: center;
	}

	.list-column {
		width: 30%;
		height: 500px;
		margin: 10px 10px;
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
</style>