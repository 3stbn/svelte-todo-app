<script>
  import { fly } from "svelte/transition";
  export let todo;
  export let remove;
</script>

<div
  class="notification"
  class:is-success={todo.completed}
  transition:fly={{ duration: 2000, x: 200 }}>
  <input type="checkbox" bind:checked={todo.completed} />
  {todo.title}
  <button class="delete" on:click={() => remove(todo)} />
</div>
