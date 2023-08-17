<script lang="ts">
	import type { PageData } from './$types';
	import Textfield from '@smui/textfield';
	import HelperText from '@smui/textfield/helper-text';
	import { onMount } from 'svelte';

	export let data: PageData;

	type Todo = {
		id: number;
		title: string;
		text: string;
	};

	let todo: Todo = {
		id: 1,
		title: 'タイトル',
		text: '詳細'
	};

	// ローカルストレージからTODOを取得
	const getTodo = (): Todo => {
		const todos = JSON.parse(localStorage.getItem('todos') ?? '') as Todo[];
		const todo = todos.find((v) => v.id === data.id);
		if (!todo) throw new Error('TODOが存在しません。');
		return todo;
	};

	onMount(() => {
		todo = getTodo();
	});
</script>

<svelte:head>
	<title>TODO</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>TODO LIST</h1>
	<div class="field">
		<Textfield bind:value={todo.title} label="タイトル">
			<HelperText slot="helper">TODOのタイトルを入力してください。</HelperText>
		</Textfield>
		<Textfield textarea bind:value={todo.text} label="詳細">
			<HelperText slot="helper">TODOの詳細を入力してください</HelperText>
		</Textfield>
	</div>
</section>

<style>
	.field {
		display: grid;
	}
</style>
