<script>
	import TodoList from './TodoList.svelte';
	let todoList = [];
	
	let todo = {
		name: "",
		isDone: false,
	};
	
	function addTodo() {
		if (!todo.name) return;
		
		todoList = [...todoList, todo];
		todo = { name: "", isDone: false };
	};

	function onKeyPress(e) {
		if (e.charCode === 13)
			return addTodo();
	}
	
	function removeTodo(index) {
		todoList = [
			...todoList.slice(0, index),
			...todoList.slice(index + 1, todoList.length),
		];
	};
	
	function setDone(index) {
		todoList[index] = {
			...todoList[index],
			isDone: !todoList[index].isDone
		};
	}
	
	$: console.log(todoList);
</script>

<div class="flex flex-col">
	<h1 class="text-4xl text-center my-4 font-mono">TodoList App</h1>
	<div class="flex flex-row justify-center space-x-2 border mx-auto py-4 px-12 rounded-lg shadow-lg">
		<input
			bind:value={ todo.name }
			type="text"
			name="todo"
			class="border rounded-md px-2 py-2 focus:outline-none"
			on:keypress={ onKeyPress }
		/>
		<button
			on:click={ addTodo }
			disabled={ !todo.name }
			class="border rounded-md px-6 bg-green-400 hover:bg-green-500 active:bg-green-600 text-white"
		>
			+
		</button>
	</div>
	<ul class="flex flex-col space-y-5 mt-6 mx-auto">
		{#each todoList as todo, index}
		<li
			on:click={ () => setDone(index) }
			class="flex flex-row space-x-2 mr-36 border shadow-md rounded-lg px-4 py-1 hover:scale-110 cursor-pointer"
		>
			<button
				on:click={ () => removeTodo(index) }
				class="bg-red-500 hover:bg-red-600 active:bg-red-700 text-white text-white rounded-md px-2 shadow-sm"
			>
				-
			</button>
			<p class={ todo.isDone ? "line-through" : "" }>
				{ todo.name }
			</p>
		</li>
	{/each}
	</ul>
</div>