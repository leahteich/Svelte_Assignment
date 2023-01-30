<!-- Note: See Nested.svelte for additional code and comments :) -->

<script>
  import { afterUpdate } from 'svelte';
  import Nested from './Nested.svelte';
  afterUpdate(() => {
    document.querySelector('.js-todo-input').focus();
  });
  let todoItems = [];
  let newTodo = '';
  // #Component: we use these functions to introduce modularity 
  function addTodo() {
    newTodo = newTodo.trim();
    // #controlFlow an if statement 
    if (!newTodo) return;
    // #Component: We can think of the todo const as a "reusable" component with variable options
    const todo = {
      // #Properties: text, checked, and id are properties of a todo  
      text: newTodo,
      checked: false,
      id: Date.now(),
    };
    // #Properties: spreading an array of properties instead of specifying each one
    todoItems = [...todoItems, todo];
    newTodo = '';
  }
  // #Component for the same reason as other functions!  
  function toggleDone(id) {
    // #Properties: We adjust the checked property with the below
    const index = todoItems.findIndex(item => item.id === Number(id));
    todoItems[index].checked = !todoItems[index].checked;
  }
  // #Component once again :) 
  function deleteTodo(id) {
    todoItems = todoItems.filter(item => item.id !== Number(id));
  }
</script>

<main>
  <Nested/> <!-- #Component. This is a starter toolbar which I imported from another file.-->
  <div class="container">
    <h1 class="app-title"> Svelte Todo</h1>
    <ul class="todo-list">
      <!-- #controlFlow: we iterate through the items using #each-->
      {#each todoItems as todo (todo.id)}
        <!-- #controlFlow: checking if todo is checked -->
        <li class="todo-item {todo.checked ? 'done' : ''}">
          <input id={todo.id} type="checkbox" />
          <!-- #Reactive: When you click the box, a tick marks appears to mark that it is done -->
          <!-- #eventHandler: Use of the on keyword to react to a click rather than explicit funciton call -->  
          <label for={todo.id} class="tick" on:click={() => toggleDone(todo.id)}></label>
          <span>{todo.text}</span>
          <!-- #Reactive: you can delete items from the list with the below button -->
          <!-- #eventHandler: Use of the on keyword to react to a click rather than explicit funciton call -->  
          <button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
            <svg><use href="#delete-icon"></use></svg>
          </button>
        </li>
      {/each}
    </ul>
    <div class="empty-state">
      <!-- #Reactive: the below tags disappear on the DOM after you add an item -->
      <h2 class="empty-state__title">Add your first item</h2>
      <p class="empty-state__description"> What are your goals for CS178?</p>
    </div>
    <!-- #eventHandler: Use of the on keyword to react to submission rather than explicit funciton call -->  
    <form on:submit|preventDefault={addTodo}>
      <input class="js-todo-input" type="text" aria-label="Enter a new todo item" placeholder="E.g. Become a better programmer" bind:value={newTodo}>
    </form>
  </div>
</main>
