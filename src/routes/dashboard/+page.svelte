<script>
    import { authHandlers, authStore } from "../../store/store";
    import { getDoc, doc, setDoc } from "firebase/firestore";
    import { db } from "../../lib/firebase/firebase";


    let todoList = [];
    let currentTodo = '';
    let error = false;


    authStore.subscribe((curr) => {
        todoList = curr.data.todos

    })

    const addTodo = () => {
        error = false;
        if(!currentTodo){
            error = true;
        }
        todoList = [...todoList, currentTodo]
        currentTodo = '';
    }

    const editTodo = (index) => {
        let newTodoList = [...todoList].filter((val, i) => {
            return i !== index;
        })
        currentTodo = todoList[index]
        todoList = newTodoList

    }

    const removeTodo = (index) => {
        let newTodoList = [...todoList].filter((val, i) => {
            return i !== index;
        })
        todoList = newTodoList

    }

    async function saveTodos() {
        console.log('Saving...')
        try {
            const userRef = doc(db, 'users', $authStore.user.uid)
            await setDoc(userRef, {
                    todos:todoList,
            }, {merge:true}) //not override email
        } catch(err){
            console.log('There was an error saying your information')
        }
    }
</script>


{#if !$authStore.loading}


<div class="mainContainer">
    <div class="headerContainer">
        <h1>Todo List</h1>
        <div class="headerBtns">
            <button on:click={saveTodos}><i class="fa-solid fa-floppy-disk"></i> Save</button>
            <button on:click={authHandlers.logout}><i class="fa-solid fa-right-from-bracket"></i>Log out</button>
        </div>
    
    </div>
    <main>
        {#if todoList.length === 0} 
        <p>
            You have no thing to do!
        </p>
        {/if}

        {#each todoList as todo, index}
            <div class="todo">
                <p> {index + 1}. {todo}</p>
        
                <div class="actions">
                    <button on:click={ () => {editTodo(index)}} ><i  class="fa-solid fa-pen-to-square"></i></button> 
                    <button on:click={ () => {removeTodo(index)}}><i class="fa-solid fa-trash"></i></button>
    
                </div>
            </div>
        
        {/each}
    </main>

    <div class={"enterTodo" + (error ? "errorBorder" : "")}>
        <input type="text" placeholder="Enter todo" bind:value={currentTodo}/>
        <button on:click={addTodo}>Add</button>

    </div>

</div>
{/if}
<style>
    .mainContainer {
        display: flex;
        flex-direction: column;
        gap:24px;
        min-height:100vh;
        padding:24px;
        width:100%;
        max-width: 1000px;
        margin: 0 auto;
    }

    .headerBtns {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap:10px;
    }

    .headerContainer{
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .headerContainer button {
        background: #003c5b;
        color:white;
        padding:10px 18px;
        border:none;
        border-radius: 5px;
        font-weight: 700;
        gap:10px;
        display: flex;
        align-items: center;
    }

    .headerContainer button i {
        font-size: 1.2rem;
    }

    main {
        display: flex;
        flex-direction: column;
        gap:8px;
        flex:1;
    }

    .enterTodo {
        display: flex;
        align-items: stretch;
        border:1px solid #0891b2;
    }


    .errorBorder {
        border:1px solid red;
    }

    .todo {
        border-left:1px solid cyan;
        padding:4px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .actions {
        display: flex;
        align-items: center;
        gap:8px;
    }

    .actions i:hover {
        color:coral;
    }


    .enterTodo input {
        background: transparent;
        border:none;
        padding:4px 8px;
        color:white;
        flex:1;
        border-radius:5px;
    }

    .enterTodo input:focus {
        outline:none;
    }

    .enterTodo button {
        padding:0 14px;
        background: #003c5b;
        border:none;
        color:cyan;
        font-weight: 600;
        cursor: pointer;
    }

    .enterTodo button:hover {
        background: transparent;
    }


</style>