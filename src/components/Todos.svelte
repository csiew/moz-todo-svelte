<script>
  export let todos = [];
  let newTodoName = '';
  let filter = 'all';

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter(todo => todo.completed).length;
  $: newTodoId = totalTodos ? Math.max(...todos.map(t => t.id)) + 1 : 1;

  function addTodo() {
    todos = [
      ...todos,
      {
        id: newTodoId,
        name: newTodoName,
        completed: false
      }
    ];
  }

  function removeTodo(todo) {
    todos = todos.filter(t => t.id !== todo.id);
  }

  $: filteredTodos = () => {
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
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">

  <!-- NewTodo -->
  <form on:submit|preventDefault={addTodo}>
    <h2 class="label-wrapper">
      <label for="todo-0" class="label__lg">
        What needs to be done?
      </label>
    </h2>
    <input type="text" id="todo-0" autocomplete="off"
      class="input input__lg" bind:value={newTodoName} />
    <button type="submit" disabled="" class="btn btn__primary btn__lg">
      Add
    </button>
  </form>

  <!-- Filter -->
  <div class="filters btn-group stack-exception">
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === 'all'}
      aria-pressed={filter === 'all'}
      on:click={() => filter = 'all'}
    >
      <span class="visually-hidden">Show</span>
      <span>All</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === 'active'}
      aria-pressed={filter === 'active'}
      on:click={() => filter = 'active'}
    >
      <span class="visually-hidden">Show</span>
      <span>Active</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === 'completed'}
      aria-pressed={filter === 'completed'}
      on:click={() => filter = 'completed'}
    >
      <span class="visually-hidden">Show</span>
      <span>Completed</span>
      <span class="visually-hidden">tasks</span>
    </button>
  </div>

  <!-- TodosStatus -->
  <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>

  <!-- Todos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filteredTodos() as todo (todo.id)}
      <li class="todo">
        <div class="stack-small">
          <div class="c-cb">
            <input
              type="checkbox"
              id="todo-{todo.id}"
              checked={todo.completed}
              on:click={() => todo.completed = !todo.completed}
            />
            <label for="todo-{todo.id}" class="todo-label">
              {todo.name}
            </label>
          </div>
          <div class="btn-group">
            <button type="button" class="btn">
              Edit <span class="visually-hidden">{todo.name}</span>
            </button>
            <button type="button" class="btn btn__danger" on:click={() => removeTodo(todo)}>
              Delete <span class="visually-hidden">{todo.name}</span>
            </button>
          </div>
        </div>
      </li>
    {:else}
      <li>Nothing to do here!</li>
    {/each}
  </ul>

  <hr />

  <!-- MoreActions -->
  <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div>

</div>