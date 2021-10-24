<script lang='ts'>
  import { createEventDispatcher, onMount } from 'svelte';
  import { selectOnFocus } from '../actions';
  const dispatch = createEventDispatcher();

  export let autofocus: boolean = false;

  let name = '';
  let nameEl: HTMLElement;

  const addTodo = () => {
    dispatch('addTodo', name);
    name = '';
    nameEl.focus();
  }

  const onCancel = () => {
    name = '';
    nameEl.focus();
  };

  onMount(() => autofocus && nameEl.focus());
</script>

<form on:submit|preventDefault={addTodo} on:keydown={e => e.key === 'Escape' && onCancel()}>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">
      What needs to be done?
    </label>
  </h2>
  <input type="text" id="todo-0" autocomplete="off"
    class="input input__lg" bind:value={name} bind:this={nameEl} use:selectOnFocus />
  <button type="submit" disabled={name.length === 0} class="btn btn__primary btn__lg">
    Add
  </button>
</form>
