<script>
  import Home from './pages/Home.svelte'
	import Alert from './components/Alert.svelte'
  import { onMount } from 'svelte';

  let list = null;

  $: console.log('list', list)
  
  onMount(() => {
    list = [
      { id: 1, date: '30/11/2021', content: 'Learn more skill', status: 'Todo' },
      { id: 6, date: '11/08/2021', content: 'Create something', status: 'Doing' },
      { id: 3, date: '01/09/2021', content: 'Play game', status: 'Done' },
      { id: 5, date: '01/07/2021', content: 'Sleep', status: 'Done' },
      { id: 2, date: '31/10/2021', content: 'Work', status: 'Todo' },
      { id: 4, date: '31/10/2021', content: 'Go outside', status: 'Doing' },
    ]
  })
  
</script>

<Alert />
{#if list}
  <Home bind:list />
{/if}

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>