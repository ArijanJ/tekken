<script>
  import MoveList from "./lib/MoveList.svelte"
  import Searchbar from "./lib/Searchbar.svelte"

  let search
</script>

<main>
  <Searchbar bind:currsearch={search} />
  <MoveList {search} />
</main>