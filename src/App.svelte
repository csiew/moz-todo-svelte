<script lang='ts'>
	import Alert from './components/Alert.svelte';
	import Todos from './components/Todos.svelte';

	import { todos } from './stores';
</script>

<main>
	<Alert />
	<Todos bind:todos={$todos} />
</main>
