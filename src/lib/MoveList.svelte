<script>
  import moves from "../assets/moves.json"
  import Move from "../lib/Move.svelte"

  export let search;

  const uselessChars = [' ', ',', '/', '+']

  function filterMove(str) {
    str = str.toLowerCase()
    uselessChars.forEach(char => {
      str = str.replaceAll(char, '')
    })
    return str
  }
</script>

<div class="movelist">
  <!-- TODO: Implement search delay (250ish ms) -->

  {#if search.length < 3}
    <!-- Don't search, provide top 50 -->
    {#each { length: 50 } as _, i}
      <Move move={moves[i]} />
    {/each}

  <!-- Character only -->
  {:else if search.split(" ").length <= 1}
    {#each moves as move}
      {#if move["C"]
        .toLowerCase()
        .includes(search.toLowerCase()) || filterMove(move["M"].toLowerCase()).includes(search.toLowerCase())}
        <Move {move} />
      {/if}
    {/each}

  <!-- Character and move -->
  {:else}
    {#each moves as move}
      {#if move["C"]
        .toLowerCase()
        .includes(search.split(" ")[0].toLowerCase())}
        {#if filterMove(move["M"])
          .toLowerCase()
          .includes(search.split(" ").slice(1).join())}
          <Move {move} />
        {/if}
      {/if}
    {/each}
  {/if}
</div>
