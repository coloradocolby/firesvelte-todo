<script>
  export let name;

  import Profile from "./Profile.svelte";
  import Todos from "./Todos.svelte";

  import { auth, googleProvider } from "./firebase";
  import { authState } from "rxfire/auth";

  let user = authState(auth);

  const login = () => {
    auth.signInWithPopup(googleProvider);
  };
</script>

<!-- add stuff to head of doc since there is no index.html -->
<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/bulma@0.8.0/css/bulma.min.css" />
  <style>
    body {
      background: #eff0eb;
      font-weight: 700;
      padding: 2em 0;
    }

    .panel {
      background: white;
    }
  </style>
</svelte:head>
<main>
  <div style="display: flex; align-items: center; flex-direction: column">

    <h1 class="title">{name}</h1>

    {#if $user}
      <Profile
        displayName={$user.displayName}
        photoURL={$user.photoURL}
        on:logout={() => auth.signOut()} />

      <Todos uid={$user.uid} />
    {:else}
      <button on:click={login} class="button is-info">Login with Google</button>
    {/if}

  </div>

</main>
