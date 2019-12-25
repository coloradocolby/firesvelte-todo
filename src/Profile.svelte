<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  const logout = () => {
    dispatch("logout");
  };

  export let displayName;
  export let photoURL;
</script>

<figure class="image is-96x96" style="margin-bottom:1em">
  <img src={photoURL} class="is-rounded" alt="user avatar" />
</figure>
<p class="is-5 has-text-weight-bold" style="margin-bottom: 1.25em">
  {displayName}
</p>
<button
  on:click={logout}
  class="button is-danger is-small"
  style="position:absolute; top: 1em; right: 1em; margin: 1.25em">
  Logout
</button>
