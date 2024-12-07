<script lang="ts">
  import { browser } from "$app/environment";
  import { onMount, onDestroy } from "svelte";

  let note: HTMLDivElement;
  let topDiv: HTMLDivElement;
  let bottomDiv: HTMLDivElement;
  export let noteText;
  let isOpen = false;
  let height = 0;

  // Update the height of the bottom div dynamically
  const updateHeight = () => {
    height = note?.offsetHeight || 0;
  };

  const handleClick = (isHovering: boolean) => {
    // Adjust the top and bottom div positions based on the hover state
    if (isHovering) {
      topDiv.style.bottom = `${height}px`;
      bottomDiv.style.top = `calc(100vh - ${height}px)`;
      // set timeout
      setTimeout(() => {
        isOpen = true;
      }, 300);
    }
  };

  const closeMenu = () => {
    if (isOpen) {
      topDiv.style.bottom = "0px";
      bottomDiv.style.top = "100vh";
      isOpen = false;
    }
  };

  // Ensure the height is updated on load and resize
  if (browser) {
    window.addEventListener("resize", updateHeight);

    onMount(() => updateHeight());
    onDestroy(() => window.removeEventListener("resize", updateHeight));

    document
      .querySelector("span[id='de']")
      .addEventListener("click", () => handleClick(true));

    document
      .querySelector("div[id='topDiv']")
      .addEventListener("click", () => closeMenu());
  }
</script>

<!-- Top div -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div
  bind:this={topDiv}
  id="topDiv"
  class="h-screen w-screen p-[40px] absolute bottom-0 left-0 ease-in-out duration-300"
>
  <p class="monument w-[87.5%]">
    <slot></slot>
  </p>
</div>

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
