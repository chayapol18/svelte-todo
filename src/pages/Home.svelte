<script>
  import { onMount } from 'svelte'
  import Heading from '../components/Heading.svelte'
  import TasksList from '../components/TasksList.svelte'
  import AddNewTask from '../components/AddNewTask.svelte'
  import { alert } from '../stores.js'
	export let list = [];
  let isOpen = false;

  onMount(() => {
    list.sort(function (a, b) {
      return b.id - a.id;
    })
    console.log('onMount list', list);
  })

	const changeStatus = ({id, status}) => {
		list = list.map((item) => {
      if (id == item.id) {
        alert.set(`${item.content} has been changed status to ${status}`)
        return {...item, status}
      } else {
        return item
      }
			// return id == item.id ? {...item, status} : item
		})
	}

  const addTask = (detail) => {
    list = [ {id: list.length > 0 ? list[0].id + 1 : 1, ...detail}, ...list ]
    isOpen = false
    $alert = `${detail.status} : '${detail.content}' has been added`
  }

	const editTask = (detail) => {
		list = list.map((item) => {
			return detail.id == item.id ? {...item, content: detail.content || item.content, date: detail.date || item.date} : item
		})
    
    alert.set(`${detail.content} has been updated`)
	}

  const copyTask = (detail) => {
    list = [ {...detail, id: list.length > 0 ? list[0].id + 1 : 1}, ...list ]
    $alert = `This ${detail.status} : '${detail.content}' is copy already.`
  }

	const deleteTask = (detail) => {
		list = list.filter((item) => {
			return item.id !== detail.id
		})
    $alert = `${detail.content} has been deleted.`
	}

  const deleteAllTasks = () => {
    $alert = `${list.length} tasks have been deleted`
		list = []
	}

  const openAddTask = (status) => {
    isOpen = status
  }

</script>
<div class="bg-gray-100 h-full">
  <!-- Head -->
  <Heading on:openAddTask={e => openAddTask(e.detail)} {isOpen} />

  <!-- add new task -->
  {#if isOpen == true}
    <AddNewTask on:addTask={e => addTask(e.detail)} />
  {/if}

  <!-- all tasks list -->
  <TasksList bind:list
    on:changeStatus={e => changeStatus(e.detail)}
    on:editTask={e => editTask(e.detail)} 
    on:deleteTask={e => deleteTask(e.detail)}
    on:deleteAllTasks={deleteAllTasks}
    on:copyTask={e => copyTask(e.detail)}
  />


</div>

<style>

</style>