<script lang='ts'>
  import type { TodoType } from '../types/todo.type';

  export let todos: TodoType[];

  let headingEl: HTMLElement;

  let totalTodos: number;
  let completedTodos: number;
  $: totalTodos = todos.length;
  $: completedTodos = todos.filter(todo => todo.completed).length;

  export const focus = () => headingEl.focus();
</script>

<h2 id="list-heading" bind:this={headingEl} tabindex={-1}>{completedTodos} out of {totalTodos} items completed</h2>
