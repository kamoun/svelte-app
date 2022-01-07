<script>
import { element } from "svelte/internal";


const authors = ['Ray Bradbury', 'John Steinbeck', 'Harper Lee', 'Hermann Melville', 'malik'];
/*[
    {id: 1, name:'Louisa May Alcott'},
    {id: 2, name:'John Steinbeck'},
    {id: 3, name:'Harper Lee'},
    {id: 4, name:'Hermann Melville'},
    {id: 5, name:'malik'}
]*/

//$: console.log({author})
let selected
function handleSelectAuthor() {
    console.log(selected)
}
</script>

<select name="authors-list" id="author" bind:value={selected} on:change="{handleSelectAuthor}" >
    <option value="">--Please choose an option--</option>
    {#each authors as author}
        <option value={author}>{author}</option>
    {/each}
</select>