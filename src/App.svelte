<script>
  import Todo from "./Todo.svelte";
  import { onMount } from "svelte";

  let input = "";
  let todos = [];
  let loading = false;

  onMount(async () => {
    loading = true;
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/user/1/todos"
    );
    todos = await response.json();
    loading = false;
  });

  $: pendings = todos.filter(t => !t.completed);
  $: completed = todos.filter(t => t.completed);

  function add() {
    const todo = {
      id: todos.length + 1,
      title: input,
      completed: false
    };
    todos = [todo, ...todos];
    input = "";
  }

  function remove(todo) {
    todos = todos.filter(t => t.id !== todo.id);
  }
</script>

<div class="section">
  <div class="field has-addons">
    <input type="text" class="input" bind:value={input} />
    <button class="button is-success" on:click={add}>Add todo !!</button>
  </div>
</div>

{#if loading}
  <div class="section">
    <p>Loading ...</p>
  </div>
{:else if todos.length > 0}
  <div class="section columns">
    <div class="column">
      {#each pendings as todo (todo.id)}
        <Todo bind:todo {remove} />
      {/each}
    </div>
    <div class="column">
      {#each completed as todo (todo.id)}
        <Todo bind:todo {remove} />
      {/each}
    </div>
  </div>
{:else}
  <div class="section">
    <p>To add a new todo please write into the input and click add</p>
  </div>
{/if}
