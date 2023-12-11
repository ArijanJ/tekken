<script>
  import { onMount } from "svelte"

  export let move
  let bg

  let gifOpen = false

  onMount(() => {
    if (move["G"] == "None") {
      bg.style.backgroundColor = "#24283b"
      bg.style.cursor = "not-allowed"
    }
  })

  function showgif() {
    if (move["G"] == "None") return
    if (gifOpen) {
      gifOpen = false
      removeGif()
    }

    gifOpen = true

    let div = document.createElement("div")

    div.style.position = "absolute"
    let rect = bg.getBoundingClientRect()

    div.style.top = rect.bottom + window.scrollY + "px"
    div.style.left = rect.left + "px"

    div.style.zIndex = "9999"
    div.style.overflow = "hidden"
    let iframe = document.createElement("iframe")
    iframe.style.width = +bg.offsetWidth + "px"
    iframe.style.height =
      (bg.offsetWidth / 16) * 9 /* + 44 */ /* damn you gfycat banner */ + "px"

    if (move["G"].includes("gfycat")) {
        let ss = move["G"].split('/')
        let id = ss[ss.length-1].replace(".gif", '')
        iframe.src = `https://web.archive.org/web/0if_/https://thumbs.gfycat.com/${id}-mobile.mp4`
    }

    // TODO: Make sense of these events

    function removeGif() {
      div.remove()
      document.removeEventListener("click", clickHandler)
      return
    }

    function escapeHandler() {
      removeGif()
      removeEventListener("keydown", this) // Does this even work?
      return
    }
    document.addEventListener("keydown", escapeHandler)

    function clickHandler() {
      removeGif()
      removeEventListener("click", this)
    }
    setTimeout(() => {
      document.addEventListener("click", clickHandler)
    }, 500)

    div.appendChild(iframe)

    document.body.appendChild(div)
  }
</script>

<div bind:this={bg} class="move" style="width: 20rem" on:click={showgif} on:keydown>
  <div style="flex: 1">
    <div style="color: #e0af68;">{move["C"].trim()}</div>
    <div style="color: #73daca;">Move: {move["M"].trim()}</div>
    <div style="color: #b4f9f8;">Hits: {move["P"].trim()}</div>
    <div style="color: #aafaf8">
      {#if move["T"].length != 0}
        Properties: {move["T"]}
      {:else}
        Properties: None
      {/if}
    </div>
  </div>
  <div style="flex: 1">
    <div style="color: #bb9af7">Startup: {move["S"].trim()}</div>
    <div style="color: #f7768e">Block: {move["B"].trim()}</div>
    <div style="color: #9ece6a">Hit: {move["H"].trim()}</div>
    <div style="color: #7aa2f7">CH: {move["CH"].trim()}</div>
  </div>
</div>
