<script>
  import { createEventDispatcher } from "svelte";
  import { fade } from "svelte/transition";
  const dispatch = createEventDispatcher();
  let container;
</script>

<div
  bind:this={container}
  class="fixed w-full h-full bg-slate-200/75 top-0 left-0 z-50"
  on:click={(e) => {
    if (e.target !== container) {
      return;
    }
    dispatch("close");
  }}
  transition:fade
>
  <slot />
</div>
