<script>
    export let index;
    export let todo;
</script>

<div class="todo">
    <p> {index + 1}. {todo}</p>

    <div class="actions">
        <button on:click={ () => {editTodo(index)}} ><i  class="fa-solid fa-pen-to-square"></i></button> 
        <button on:click={ () => {removeTodo(index)}}><i class="fa-solid fa-trash"></i></button>

    </div>
</div>

<style>

</style>