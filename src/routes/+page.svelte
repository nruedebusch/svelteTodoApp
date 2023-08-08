<script>
  import { fade, fly } from "svelte/transition";
  let toDoList = [];

  function setEditing(i, isEditing) {
    toDoList[i].editing = isEditing;
  }
  function deleteTodo(i) {
    toDoList.splice(i, 1);
    toDoList = [...toDoList];
  }

  function addTodo() {
    toDoList = [
      ...toDoList,
      { content: textInput, editing: false, checked: false },
    ];
    textInput = "";
  }

  let textInput = "";
</script>

<!-- Stylin -->
<svelte:head>
  <link
    rel="stylesheet"
    href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css"
  />
</svelte:head>

<div style="margin: 0 auto; padding: 20px; width: 700px">
  <h2 style="text-align: center;">Todo List</h2>
  <p>Enter a task</p>
  <div style="display: flex;">
    <input
      type="text"
      bind:value={textInput}
      on:keyup={(event) => event.key === "Enter" && addTodo()}
    />
    <button style="width: 200px;" on:click={addTodo}>Add</button>
  </div>
</div>

{#each toDoList as toDo, i (toDo)}
  <div
    in:fly={{ y: 20, duration: 200 }}
    out:fade={{ duration: 300 }}
    style="display: flex; align-items:baseline; width: 700px; margin: 0 auto; padding: 20px;"
  >
    {#if toDo.editing}
      <input type="text" bind:value={toDo.content} />
    {:else}
      <input type="checkbox" bind:checked={toDo.checked} />
      <h4 style="flex-grow: 1;">{toDo.content}</h4>
    {/if}
    <div style="display: flex; gap:.5rem">
      {#if toDo.editing}
        <button on:click={() => setEditing(i, false)}>Save</button>
      {:else}
        <button on:click={() => setEditing(i, true)}>Edit</button>
      {/if}
      <button on:click={() => deleteTodo(i)}>Delete</button>
    </div>
  </div>
{/each}
