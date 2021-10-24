<script>
  import { fly, fade } from 'svelte/transition';
  import TodosStatus from './TodosStatus.svelte';
  import FilterButton from './FilterButton.svelte';
  import Todo from './Todo.svelte';
  import NewTodo from './NewTodo.svelte';
  import MoreActions from './MoreActions.svelte';

  export let todos = [];
  let filter = 'all';
  let todosStatus;

  $: newTodoId = todos.length ? Math.max(...todos.map(t => t.id)) + 1 : 1;

  function addTodo(name) {
    todos = [
      ...todos,
      {
        id: newTodoId,
        name: name,
        completed: false
      }
    ];
  }

  function removeTodo(todo) {
    todos = todos.filter(t => t.id !== todo.id);
    todosStatus.focus();
  }

  function updateTodo(todo) {
    const i = todos.findIndex(t => t.id === todo.id);
    todos[i] = { ...todos[i], ...todo };
  }

  const filterTodos = (filter, todos) => {
    switch (filter) {
      case 'active': {
        return todos.filter(t => !t.completed);
      }
      case 'completed': {
        return todos.filter(t => t.completed);
      }
      default: {
        return todos;
      }
    }
  }

  const checkAllTodos = (completed) => todos = todos.map(t => ({ ...t, completed: completed }));
  const removeCompletedTodos = () => todos = todos.filter(t => !t.completed);
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">

  <!-- NewTodo -->
  <NewTodo autofocus on:addTodo={e => addTodo(e.detail)} />

  <!-- Filter -->
  <FilterButton bind:filter />

  <!-- TodosStatus -->
  <TodosStatus bind:this={todosStatus} {todos} />

  <!-- Todos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li
        class="todo"
        in:fly={{ x: -100, duration: 500 }}
        out:fly={{ x: 100, duration: 500 }}
      >
        <Todo
          bind:todo
          on:remove={e => removeTodo(e.detail)}
          on:update={e => updateTodo(e.detail)}
        />
      </li>
    {:else}
      <li in:fade={{ duration: 500 }}>Nothing to do here!</li>
    {/each}
  </ul>

  <hr />

  <!-- MoreActions -->
  <MoreActions
    {todos}
    on:checkAll={e => checkAllTodos(e.detail)}
    on:removeCompleted={removeCompletedTodos}
  />

</div>