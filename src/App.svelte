<script>
  import Header from "./components/Header.svelte";
  import List from "./components/List.svelte";
</script>

<style>
  .container {
    width: 500px;
    background: #009579;
  }

  :global(.container *) {
    font-family: "Quicksand", sans-serif;
  }
</style>

<div class="container">
  <Header />
  <List />
</div>
