<script>
  import { onMount } from "svelte";
  import BookGrid from "./BookGrid.svelte";
  import Button from "../common/Button.svelte";
  import { httpGet } from "../common/api.js";
  import Filter from "../common/Filter.svelte";
  import { library } from "../stores/librarystore";
  console.log("library", $library);

  let books = [];
  let author = "";
  onMount(async function () {
    const { data } = await httpGet("/?_sort=id&_order=desc");
    books = data;
  });

  async function handleAuthorSelect(data) {
    console.log("library", data.detail.data);
    author = data.detail.data;
    console.log("author", author);
    if (author !== "") {
      const { data } = await httpGet("/?_sort=id&_order=desc&author=" + author);
      if (data) {
        console.log("ok", data);
        $: books = data;
      }
    } else {
      const { data } = await httpGet("/?_sort=id&_order=desc");
      if (data) {
        $: books = data;
      }
    }
  }
</script>

<svelte:head>
  <title>My library</title>
</svelte:head>
<header>
  <span class="preamble">Welcome to the</span>
  <h1>Library</h1>
</header>
<p class="greeting">This is a library for the preople.</p>
<Button to="/create">Add Book</Button>
<Filter on:author-select={handleAuthorSelect} />
<BookGrid {books} />

{#each $library as lib}
  <p>{lib.name}</p>
{/each}

<style>
  header {
    margin: var(--spacingMedium) 0 var(--spacingLarge) 0;
    text-transform: uppercase;
  }
  .preamble {
    display: block;
  }
  h1 {
    font-size: var(--typeSizeXXLarge);
    font-weight: var(--typeWeightBold);
    line-height: var(--typeLineHeightTight);
  }
  .greeting {
    font-size: var(--typeSizeSmall);
  }
</style>
