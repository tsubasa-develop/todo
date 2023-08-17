<script lang="ts">
	import Textfield from '@smui/textfield';
	import HelperText from '@smui/textfield/helper-text';
	import Button, { Label } from '@smui/button';
	import List, { Item, Meta, Label as ListLabel } from '@smui/list';
	import Checkbox from '@smui/checkbox';
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';

	type Todo = {
		id: number;
		title: string;
		text: string;
	};

	let todos: Todo[] = [];
	let title = '';
	let text = '';
	let selected: number[] = [];

	const addTask = () => {
		if (title === '') return;
		todos = [...todos, { id: todos.length + 1, title, text }];
		saveTodos();
		clearTodos();
	};

	const deleteTask = () => {
		// todosから、selectedに含まれるidを除外する
		todos = todos.filter((todo) => !selected.includes(todo.id));
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

	const clearTodos = () => {
		if (!browser) return;
		title = '';
		text = '';
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
	<h1>TODO LIST</h1>
	<div class="field">
		<Textfield bind:value={title} label="タイトル">
			<HelperText slot="helper">TODOのタイトルを入力してください。</HelperText>
		</Textfield>
		<Textfield textarea bind:value={text} label="詳細">
			<HelperText slot="helper">TODOの詳細を入力してください</HelperText>
		</Textfield>
	</div>
	<Button on:click={addTask} variant="raised">
		<Label>追加</Label>
	</Button>

	<List class="demo-list" checkList on:SMUIList:selectionChange={() => {}}>
		{#each todos as todo (todo.id)}
			<Item>
				<ListLabel>{todo.title}</ListLabel>
				<Meta>
					<Checkbox bind:group={selected} value={todo.id} />
				</Meta>
			</Item>
		{/each}
	</List>
	<Button on:click={deleteTask} variant="raised">
		<Label>削除</Label>
	</Button>
</section>

<style>
	.field {
		display: grid;
	}
</style>
