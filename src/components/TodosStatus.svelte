<script>
  export let todos;

  let headingEl;

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter(todo => todo.completed).length;

  export const focus = () => headingEl.focus();
</script>

<h2 id="list-heading" bind:this={headingEl} tabindex="-1">{completedTodos} out of {totalTodos} items completed</h2>
