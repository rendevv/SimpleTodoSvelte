<script lang="ts">
    import { onMount } from "svelte";

    interface todolist{
        content: string,
        editing: boolean,
        checked: boolean
    }

    let toDoList: todolist[] = [];
    let textInput: string = "";

    onMount(() => {
        const storedToDoList = localStorage.getItem("toDoList");
        if (storedToDoList) {
            toDoList = JSON.parse(storedToDoList);
        }
    });

    function addToDo() {
        toDoList = [...toDoList, { content: textInput, editing: true, checked: false }];
        updateLocalStorage();
        textInput = ""; 
    }

    function setEditing(i: number, isEditing: boolean) {
        toDoList[i].editing = isEditing;
        updateLocalStorage();
    }

    function setDelete(i: number) {
        toDoList.splice(i, 1);
        toDoList = toDoList;
        updateLocalStorage();
    }

    function updateLocalStorage() {
        localStorage.setItem("toDoList", JSON.stringify(toDoList));
    }
</script>

<div class="todo">
    <h2>Todo List</h2>
    <p>enter your todo here</p>
    <div class="todo_form">
        <input type="text" bind:value={textInput}>
        <button on:click={addToDo}>Add +</button>
    </div>
</div>

{#each toDoList as todo, i}
<div class="list">
    {#if todo.editing}
    <input type="text" bind:value={todo.content}>
    {:else}
    <input type="checkbox" bind:checked={todo.checked}>
    <h4>{todo.content}</h4>
    {/if}
    <div class="list_btn">
        {#if todo.editing}
            <button on:click={() => setEditing(i, false)}>Save</button>
        {:else}
        <button on:click={() => setEditing(i, true)}>Edit</button>
        <button on:click={() => setDelete(i)}>Delete</button>
        {/if}
    </div>
</div>
{/each}

<style lang="scss">
    .todo {
        margin: 0 auto;
        padding: 20px;
        width: 700px;

        h2 {
            text-align: center;
        }

        p {
            font-size: large;
        }

        &_form {
            display: flex;

            input {
                padding: 1rem;
                width: 100%;
            }

            button {
                background-color: lightseagreen;
                color: white;
                font-weight: 900;
                border-radius: 3px;
                width: 200px;
            }
        }
    }

    .list {
        display: flex;
        align-items: baseline;
        padding: 20px;
        width: 700px;
        gap: 2rem;
        margin: 0 auto;

        h4 {
            flex-grow: 1;
        }

        input {
            padding: 1rem;
        }

        &_btn {
            display: flex;
            gap: 1rem;

            button {
                background-color: lightseagreen;
                color: white;
                font-weight: 900;
                border-radius: 3px;
                padding: 1rem;
            }
        }
    }
</style>
