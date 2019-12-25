<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  const remove = () => {
    dispatch("remove", { id });
  };

  const toggleStatus = () => {
    let newStatus = !complete;
    dispatch("toggle", {
      id,
      newStatus
    });
  };

  export let id;
  export let text;
  export let complete;
</script>

<style>
  .complete {
    text-decoration: line-through;
    color: green;
  }
</style>

<div style="width: 100%; display:flex; justify-content: space-between">
  <div>
    <span class:complete>{text}</span>
  </div>

  <div>
    {#if !complete}
      <span class="icon" style="cursor: pointer" on:click={toggleStatus}>
        ✔️
      </span>
    {:else}
      <span class="icon" style="cursor: pointer" on:click={toggleStatus}>
        ❌
      </span>
    {/if}
    <span class="icon" style="cursor: pointer" on:click={remove}>🗑</span>
  </div>
</div>
