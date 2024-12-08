<script lang="ts">
  import { browser } from "$app/environment";
  import { onMount, onDestroy } from "svelte";

  let note: HTMLDivElement;
  let topDiv: HTMLDivElement;
  let bottomDiv: HTMLDivElement;
  export let noteText: string;
  let height = 0;

  // Dynamically update the height of the bottom div
  const updateHeight = () => {
    if (note) {
      height = note.offsetHeight || 0;
    }
  };

  // Ensure the height is updated on load and resize
  if (browser) {
    window.addEventListener("resize", updateHeight);

    onMount(() => {
      updateHeight();
    });

    onDestroy(() => {
      window.removeEventListener("resize", updateHeight);
    });
  }

  const handleClick = () => {
    // Adjust the top and bottom div positions based on click
    topDiv.style.bottom = `${height}px`;
    bottomDiv.style.top = `calc(100vh - ${height}px)`;
  };
</script>

<!-- Top div -->
<div
  bind:this={topDiv}
  id="topDiv"
  class="h-screen w-screen p-[40px] absolute bottom-0 left-0 ease-in-out duration-300"
>
  <p class="monument w-[87.5%]">
    <slot></slot>
    <!-- Handle click on the span directly inside this component -->
    {#if noteText}
      <span
        id="de"
        on:click={(event) => {
          event.stopPropagation();
          handleClick();
        }}
      >
        Notes
      </span>
    {/if}
  </p>
</div>

<!-- Bottom div -->
<div
  bind:this={bottomDiv}
  class="absolute top-[100vh] ease-in-out duration-300 w-screen bg-[#FF0A0E]"
  bind:this={note}
>
  <div class="w-[87.5%]">
    <p class="p-[40px] monument flex">
      [*] {noteText}
    </p>
  </div>
</div>
