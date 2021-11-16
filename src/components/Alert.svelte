<script>
  import { onDestroy } from 'svelte'
  import { alert } from '../stores.js'

  export let ms = 3000;
  let visible;
  let timeout;

  const onMessageChange = (message, ms) => {
    clearTimeout(timeout)
    if (!message) {
      visible = false
    } else {
      visible = true                                             
      if (ms > 0) timeout = setTimeout(() => visible = false, ms) 
    }
  }
  $: onMessageChange($alert, ms) 

  onDestroy(()=> { 
    // console.log('Alert is destroyed already');
    clearTimeout(timeout)
  })

</script>

{#if visible}
  <div role="alert" on:click={() => visible = false} class="mt-4">
    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-info-circle" viewBox="0 0 16 16">
      <path d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
      <path d="m8.93 6.588-2.29.287-.082.38.45.083c.294.07.352.176.288.469l-.738 3.468c-.194.897.105 1.319.808 1.319.545 0 1.178-.252 1.465-.598l.088-.416c-.2.176-.492.246-.686.246-.275 0-.375-.193-.304-.533L8.93 6.588zM9 4.5a1 1 0 1 1-2 0 1 1 0 0 1 2 0z"/>
    </svg>
    <p class="m-auto">{ $alert }</p>
  </div>
{/if}


<style>
div {
  position: fixed;
  cursor: pointer;
  margin: auto;
  left: 0;
  display: flex;
  align-items: center;
  border-radius: 0 0.2rem 0.2rem 0;
  /* background-color: #777; */
  background: linear-gradient(45deg, #000000, #434343);;
  color: #fff;
  font-weight: 700;
  padding: 0.5rem 1.4rem;
  font-size: 14px;
  z-index: 100;
  opacity: 95%;
}
div p {
  color: #fff;
}
div svg {
  height: 1.6rem;
  fill: currentColor;
  width: 1.4rem;
  margin-right: 0.5rem;
}
</style>
