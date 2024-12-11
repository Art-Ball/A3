<script>
  import {onMount} from 'svelte';
  import { text } from "@sveltejs/kit";
  import  bkg  from "$lib/images/pain.svg"; 
  import trashCan from"$lib/images/TrashcanIcon.svg";
  import redCan from "$lib/images/TrashcanIconRED.svg";
let todoItem = $state('');
let todoList = $state([]);
let doneList = $state([]);
let storedList;

onMount(() =>{ 
     storedList = localStorage.getItem('storedList');
     if (storedList){
          todoList = ( JSON.parse(storedList));
     }
})

function updateList() {
     return storedList = localStorage.setItem('storedList', JSON.stringify
          (todoList));
}
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
     updateList();
 }

 /* making  function that allows us to remove items */
function removeItem(index){ 

     todoList = todoList.toSpliced(index,1); /*  allowing us to target specific list item to get rid of it */
     updateList();
}


function nuke(){ /* clear function that puts the list items to 0 (none) */
     todoList = [];
     localStorage.clear();
}

$effect(() => {
     doneList = todoList.filter((item) => item.done);
     updateList();
})
function undoThis(item) {

     item.done = !item.done;
     updateList();
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
               <button type="button" onclick={() => removeItem(index)}> 
                    <span class="can"></span>
               </button> <!-- adding an x/ remove button that gets rid of one list item -->
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
          <button type="button" onclick={() => undoThis(item)}></button>
     </li>
     {/each}
 </div>
{/if}
</div>

<style>

.can {
     background: url('$lib/images/TrashcanIcon.svg') no-repeat transparent;
     background-size: 100% auto;
     height: 2.5em;
     width: 2em;
     display: inline-block;
     &:hover {
          background-image: url('$lib/images/TrashcanIconRED.svg');
     }
}
 
.lists{
     display: flex;
     justify-content: center;
     & > div{ 
          flex-basis: 40%;
          flex-shrink: 0;
     }
}

.done-list{
     margin-top: 8.5vw;

}
.done-list li{
     padding: 1.5em;

}

.done-list button{
     background: url('$lib/images/TrashcanIcon.svg') no-repeat transparent;
     padding: .4em;
    padding-left: 1em;
    padding-right: 1em;
    height: 2.5em;
    width: 2em;
     cursor: pointer;
     &:hover{ 
          background-image: url('$lib/images/TrashcanIconRED.svg');
   
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
     background-color: transparent;
     border-color: transparent;
     border-radius: 10px;
    
    margin-left: auto;
     cursor: pointer;

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
     border-left: none;
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
     display: flex;
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
.trashPic {
     height: 2em;
     width: 2em;
}
</style>