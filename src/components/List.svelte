<script>
  import { onMount } from "svelte";
  import { v4 as uuidv4 } from "uuid";

  import { items } from "../stores";

  import TodoApi from "../TodoApi";
  import Item from "./Item.svelte";
  import NewItem from "./NewItem.svelte";

  /**
   * Add new item to list and save to API.
   */
  function handleNewItem({ detail: text }) {
    $items = [
      {
        id: uuidv4(),
        text,
        complete: false,
      },
      ...$items,
    ];

    TodoApi.save($items);
  }

  /**
   * Update store with new data and save to API.
   */
  function handleUpdate({ detail }) {
    const index = $items.findIndex((item) => item.id === detail.id);
    $items[index] = detail;
    TodoApi.save($items);
  }

  /**
   * Delete item by ID from the store and save to API.
   */
  function handleDelete({ detail: id }) {
    $items = $items.filter((item) => item.id !== id);
    TodoApi.save($items);
  }

  let itemsSorted = [];

  $: {
    itemsSorted = [...$items];
    itemsSorted.sort((a, b) => {
      if (a.complete && b.complete) return 0;
      if (a.complete) return 1;
      if (b.complete) return -1;
    });
  }

  onMount(async () => {
    $items = await TodoApi.getAll();
  });
</script>

<style>
  .list {
    padding: 15px;
  }

  .list-status {
    margin: 0;
    text-align: center;
    color: #ffffff;
    font-weight: bold;
    font-size: 1.1em;
  }
</style>

<div class="list">
  <NewItem on:newitem={handleNewItem} />
  {#each itemsSorted as item (item)}
    <Item {...item} on:update={handleUpdate} on:delete={handleDelete} />
  {:else}
    <p class="list-status">No Items Exist</p>
  {/each}
</div>
