<script lang="ts">
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';

	type Todo = {
		id: number;
		task: string;
	};

	let todos: Todo[] = [];
	let task = '';

	const addTask = () => {
		if (task === '') return;
		todos = [...todos, { id: todos.length + 1, task: task }];
		saveTodos();
	};

	const deleteTask = (id: number) => {
		todos = todos.filter((todo) => todo.id !== id);
		saveTodos();
	};

	// ローカルストレージからTODOSを取得
	const getTodos = () => {
		if (!browser) return [];
		const todos = localStorage.getItem('todos');
		if (!todos) return [];
		return JSON.parse(todos);
	};

	// ローカルストレージにTODOSを保存する
	const saveTodos = () => {
		if (!browser) return;
		localStorage.setItem('todos', JSON.stringify(todos));
	};

	onMount(() => {
		todos = getTodos();
	});
</script>

<svelte:head>
	<title>TODO</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>todo list</h1>
	<input type="text" bind:value={task} />
	<button on:click={addTask}>追加</button>
	<ul>
		{#each todos as todo (todo.id)}
			<li>
				{todo.task}
				<button on:click={() => deleteTask(todo.id)}>Delete</button>
			</li>
		{/each}
	</ul>
</section>

<style>
</style>
