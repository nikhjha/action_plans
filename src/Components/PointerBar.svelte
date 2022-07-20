<script>
  import Layer from "./Layer.svelte";
  import { createEventDispatcher } from "svelte";
  export let pointer;
  let showDropDown = false;
  let showDeletePlan = false;
  const dispatch = createEventDispatcher();
</script>

<div
  class="w-full rounded-md  p-1 px-2 flex gap-2 text-gray-400 bg-slate-100 relative items-center before:content-[''] before:absolute before:-z-10 before:bottom-1/2 before:-left-4 before:w-6 before:h-16 before:border-l-2 before:border-b-2 before:rounded-md"
>
  <span class="material-symbols-outlined cursor-pointer"> drag_indicator </span>
  <h4 class="text-blue-600 text-base">{pointer.name}</h4>
  <span class="material-symbols-outlined cursor-pointer ml-auto text-md">
    visibility
  </span>
  <div class="relative">
    <span
      class="material-symbols-outlined cursor-pointer"
      on:click={() => {
        showDropDown = !showDropDown;
      }}
    >
      more_vert
    </span>
    {#if showDropDown}
      <div
        class="absolute top-full right-0 bg-white z-20 w-40 p-1 border border-gray-300"
      >
        <button
          class="p-1 text-sm "
          on:click={() => {
            showDropDown = false;
          }}
        >
          Edit Pointer
        </button>
        <button
          class="p-1 text-sm text-red-600 bg-red-200 w-full"
          on:click={() => {
            showDeletePlan = true;
            showDropDown = false;
          }}
        >
          Delete
        </button>
      </div>
    {/if}
  </div>
  {#if showDeletePlan}
    <Layer
      on:close={() => {
        showDeletePlan = false;
      }}
    >
      <div
        class="bg-slate-100 p-4 w-fit h-fit absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 text-black"
      >
        <h1 class="font-extrabold text-xl mb-2">Delete Pointer</h1>
        <p class="mb-2 w-96 font-normal text-xs">
          Are you sure you want to delete this learning, this step cannot be
          undone
        </p>
        <div class="flex justify-end gap-2 mt-4">
          <button
            class="flex items-center gap-2 p-2 rounded border border-red-600 text-red-600 text-sm w-24 justify-center"
            on:click={() => {
              showDeletePlan = false;
            }}
          >
            Cancel
          </button>
          <button
            class="flex items-center gap-2 p-2 ml-2 bg-blue-600 rounded text-white text-sm w-24 justify-center"
            on:click={() => {
              dispatch("delete", pointer.id);
              showDeletePlan = false;
            }}
          >
            Delete
          </button>
        </div>
      </div>
    </Layer>
  {/if}
</div>
