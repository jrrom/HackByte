<script lang="ts">

  import Login from "./lib/Login.svelte"
  let login: boolean = true;
  let token: string | null = null;

</script>

<main>
  {#if login}
    <Login bind:token={token} />
  {/if}
  {#if token}
    <h1> Omg you logged in!!! </h1>
  {/if}
</main>
