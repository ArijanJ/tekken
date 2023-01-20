<script>
  export let currsearch = "";
</script>

<div style="width: 97%">
  <input
    bind:value={currsearch}
    type="text"
    class="searchbar"
    placeholder="Look up a move"
  />
</div>
