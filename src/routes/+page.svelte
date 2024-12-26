<script>
  let todos = [];
  let newTodo = "";
  let editIndex = null; 
  let editText = ""; 

  function addTodo() {
    if (newTodo.trim()) {
      todos = [...todos, { text: newTodo, completed: false }];
      newTodo = "";
    }
  }

  function toggleCompletion(index) {
    todos[index].completed = !todos[index].completed;
  }

  function deleteTodo(index) {
    todos = todos.filter((_, i) => i !== index);
  }

  function enableEdit(index) {
    editIndex = index;
    editText = todos[index].text;
  }

  function saveEdit() {
    if (editText.trim()) {
      todos[editIndex].text = editText;
      editIndex = null;
      editText = "";
    }
  }
</script>

<main class="ml-4 justify-between bg-fuchsia-200 my-6 rounded-2xl place-content-center grid grid-cols-3">
  <h1 class="col-span-3 pb-5 pl-10 text-3xl	lace-content-center">To-Do List</h1>

  <input
    type="text"
    bind:value={newTodo}
    placeholder="What needs to be done?"
    on:keydown={(e) => e.key === "Enter" && addTodo()}
    class="col-span-2 m-10 p-4 text-2xl w-10/12 text-gray-700 bg-gray-100 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-4 focus:ring-blue-300 focus:border-blue-500 hover:bg-gray-200 transition duration-200 ease-in-out shadow-sm"
  />

  <div>
    <button 
      on:click={addTodo} 
      class="bg-purple-800 py-5 px-10 pl-5 rounded-2xl my-10 text-cyan-50 m-5 col-span-1">
      Add Todo
    </button>
  </div>

  <ul>
    {#each todos as { text, completed }, index}
      <li class="todo-item flex items-center justify-between bg-white p-4 m-7 rounded-md shadow-sm hover:shadow-lg transition-shadow duration-200 ease-in-out">
        {#if editIndex === index}
         
          <input
            type="text"
            bind:value={editText}
            placeholder="Edit todo"
            class="flex-1 text-lg p-2 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-300"
          />
          <button 
            class="bg-green-600 py-2 px-4 mx-2 rounded-lg text-white hover:bg-green-500 transition-colors duration-200"
            on:click={saveEdit}>
            Save
          </button>
        {:else}
      
          <span 
            class="text-lg cursor-pointer select-none transition-colors duration-200 ease-in-out hover:text-purple-600 
            {completed ? 'line-through text-gray-400' : 'text-gray-800'}"
            on:click={() => toggleCompletion(index)}>
            {text}
          </span>
          <div class="flex space-x-2">
            <button 
              class="bg-blue-600 py-2 px-4 rounded-lg text-white hover:bg-blue-500 transition-colors duration-200"
              on:click={() => enableEdit(index)}>
              Edit
            </button>
            <button 
              class="bg-purple-800 py-2 px-4 rounded-lg text-cyan-50 hover:bg-purple-600 transition-colors duration-200"
              on:click={() => deleteTodo(index)}>
              Delete
            </button>
          </div>
        {/if}
      </li>
    {/each}
  </ul>
</main>
