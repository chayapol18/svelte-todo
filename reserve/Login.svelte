<script>
  // import { Navigate } from 'svelte-router-spa'
  let username;
  let password;

</script>


<div>
  <div>
    Login

    <div>
      <span>Username : </span>
      <input type="text" bind:value={username}>
    </div>
    <div>
      <span>Password : </span>
      <input type="password" bind:value={password}>
    </div>
    <button>
      <!-- <Navigate to="/">Login</Navigate> -->
      Login
    </button>
  </div>
</div>