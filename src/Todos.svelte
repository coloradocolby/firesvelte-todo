<script>
  import TodoItem from "./TodoItem.svelte";
  import { db } from "./firebase";
  import { collectionData } from "rxfire/firestore";
  import { startWith } from "rxjs/operators";

  export let uid;

  let text = "";

  const query = db
    .collection("todos")
    .where("uid", "==", uid)
    .orderBy("created");

  const todos = collectionData(query, "id").pipe(startWith([]));

  const add = e => {
    e.preventDefault();
    if (text.trim() !== "") {
      db.collection("todos").add({
        uid,
        text,
        complete: false,
        created: Date.now()
      });
      text = "";
    }
  };

  const updateStatus = event => {
    const { id, newStatus } = event.detail;
    db.collection("todos")
      .doc(id)
      .update({ complete: newStatus });
  };

  const removeItem = event => {
    const { id } = event.detail;
    db.collection("todos")
      .doc(id)
      .delete();
  };
</script>

<article class="panel is-info">
  <p class="panel-heading">Todos</p>
  <div class="panel-block">

    <form on:submit={add}>
      <div class="field has-addons">
        <div class="control">
          <input
            class="input"
            type="text"
            placeholder="Enter your task here"
            maxlength="30"
            bind:value={text} />
        </div>
        <div class="control">
          <button class="button is-info">Add Task</button>
        </div>
      </div>
    </form>

  </div>

  {#each $todos as todo}
    <div class="panel-block" href="#">
      <TodoItem
        id={todo.id}
        text={todo.text}
        complete={todo.complete}
        on:remove={removeItem}
        on:toggle={updateStatus} />
    </div>
  {/each}

</article>
