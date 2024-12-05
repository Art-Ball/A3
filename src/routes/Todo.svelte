<script>
  import { text } from "@sveltejs/kit";

 
let todoItem = $state('');
let todoList = $state([]);
let doneList = $state([]);

function cap(w) { 
     return w.charAt(0).toUpperCase() + w.slice(1);
}

function addItem(event) {

     event.preventDefault();
     if (todoItem == ''){
          return;     
     }
     todoList = [...todoList,{

          text: cap(todoItem),
          done: false,
     }];

     todoItem = '';
 }

 /* making  function that allows us to remove items */
function removeItem(index){ 

     todoList = todoList.toSpliced(index,1); /*  allowing us to target specific list item to get rid of it */
}


function nuke(){ /* clear function that puts the list items to 0 (none) */
     todoList = [];
}

$effect(() => {
     doneList = todoList.filter((item) => item.done);
})
function undoThis(item) {

     item.done = !item.done;
}

$inspect('To Do List', todoList); 
$inspect('Done List', doneList); 

</script>

<div class="lists"> 
     <div class="todo-List"> 

     <form onsubmit={addItem} > 
     <input type="text" bind:value={todoItem}>
     <button type="submit">Add</button>
     </form>
   
     <h2> Stuff to do! </h2>
     <ul> 
          
          
          {#each todoList as item, index}

          <li>
               <input type="checkbox" bind:checked = {item.done}><!-- adding check boxes to list item -->
               <span class:done= {item.done}>{item.text}</span>
               <button type="button" onclick={() => removeItem(index)}>x</button> <!-- adding an x/ remove button that gets rid of one list item -->
          </li>
          {/each}

     </ul>
     <!-- if statment that runs the nuke function to clear the array/list items -->
     {#if (todoList.length === 0 )}

          <button disabled type="button">Clear</button>
          {:else}
          <button class="nukeButton"  type="button" onclick={nuke}> Clear</button>
     {/if}
</div>

     {#if (doneList.length > 0)}
<div class= "done-list">
     <h2>Stuff done</h2><!-- Items Completed! -->
     {#each doneList as item}
     <li>
          <span>{item.text}</span>
          <button type="button" onclick={() => undoThis(item)}> Remove</button>
     </li>
     {/each}
 </div>
{/if}
</div>

<style>

 
.lists{
     display: flex;
     justify-content: center;
     & > div{ 
          flex-basis: 40%;
          flex-shrink: 0;
     }
}
.done-list{
     
     margin-top: 9vw;
}
.done-list button{
     padding: .4em;
    padding-left: 1em;
    padding-right: 1em;
     cursor: pointer;
     &:hover{ 
          background-color: rgb(238, 65, 65);
     }
}

 .todo-List{ 
     margin: 2vw;
    
 } 

 .todo-List li button { 

    padding: .4em;
    padding-left:.9em;
    padding-right:.9em;
 } 

span.done { 
     color: #29020d;
     text-decoration: line-through;
}
li button{ 
     margin-left: 6vw;
     background-color: rgb(244, 244, 243);
     border-color: #29020d;
     border-radius: 10px;
    
    
     cursor: pointer;
     &:hover{ 
          background-color: rgb(240, 81, 81);
     }
}
input[type="checkbox"] { 
    height: 2vh;
     width: 3vw;
     margin: 0;
     cursor: pointer;
}
input[type="text"] { 
     padding-left: 1vw;
     padding-right: 1vw;
     padding: .7vw;
     border-top-left-radius: 3em;
     border-bottom-left-radius: 3em;
     border-color: #ffef96;
     border-style: solid;
     margin-left:10vw ;
     font-size: 17px;
     padding-left: .9vw ;

}
 
form button{
     padding-left: 1vw;
     padding-right: 1vw;
     padding: .6vw;
     border-top-right-radius: 3em;
     border-bottom-right-radius: 3em;
     border-color: #ffef96;
     border-style: solid;
     background-color: #ffef96;
     font-size: 17px;
     cursor: pointer;
     &:hover{ 
          background-color: #ffffff;
     }

}
button{ 
     font-family: "Parkinsans", sans-serif;
}

form{ 
     padding-bottom: 4vw;
}
.nukeButton { 
   margin-left: 2.4vw;
     padding-left: 5vw;
     padding-right: 5vw;
     padding-top: .5vw;
     padding-bottom: .5vw;
     border-radius: 1em;
     border-color: #29020d;
     &:hover{ 
          background-color: rgb(240, 81, 81);
     }
}
button:disabled{ 
     padding-left: 5vw;
     padding-right: 5vw;
     padding-top: .5vw;
     padding-bottom: .5vw;
     border-radius: 1em;
}

</style>