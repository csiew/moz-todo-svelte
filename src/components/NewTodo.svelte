<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let newTodoName = '';
</script>

<form on:submit|preventDefault={() => dispatch('addTodo', newTodoName)}>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">
      What needs to be done?
    </label>
  </h2>
  <input type="text" id="todo-0" autocomplete="off"
    class="input input__lg" bind:value={newTodoName} />
  <button type="submit" disabled={!newTodoName} class="btn btn__primary btn__lg">
    Add
  </button>
</form>
