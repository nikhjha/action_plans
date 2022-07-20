<script>
  import Layer from "./Layer.svelte";
  export let category;
  let showDropDown = false;
  let showDeletePlan = false;
  let showAddPointer = false;
  import { createEventDispatcher } from "svelte";
  import PointerBar from "./PointerBar.svelte";
  const dispatch = createEventDispatcher();
  let newPointName = "";
  const createPointer = () => {
    if (newPointName === "") {
      return;
    }
    dispatch("add_pointer", { category: category.id, pointer: newPointName });
    showAddPointer = false;
  };
</script>

<div
  class="w-full rounded-md  p-2 px-4 flex gap-2 text-gray-400 border border-slate-100 relative"
>
  <span class="material-symbols-outlined cursor-pointer"> drag_indicator </span>
  <h4 class="text-black">{category.title}</h4>
  <div class="absolute top-1/2 left-0 -translate-x-1/2 -translate-y-1/2">
    {#if category.open}
      <span
        class="material-symbols-outlined text-sm rounded bg-slate-100 px-1 cursor-pointer"
        on:click={() => {
          dispatch("expand", category.id);
        }}
      >
        remove
      </span>
    {:else}
      <span
        class="material-symbols-outlined text-sm rounded bg-slate-100 px-1 cursor-pointer"
        on:click={() => {
          dispatch("expand", category.id);
        }}
      >
        add
      </span>
    {/if}
  </div>
  <div class="relative ml-auto">
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
          Edit Sub-Category
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
        <h1 class="font-extrabold text-xl mb-2">Delete Sub-Category</h1>
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
              dispatch("delete", category.id);
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
{#if category.open}
  <div class="pl-8 flex flex-col gap-2">
    {#each category.pointers as pointer (pointer.id)}
      <PointerBar
        {pointer}
        on:delete={(e) => {
          dispatch("delete_pointer", {
            category: category.id,
            pointer: e.detail,
          });
        }}
      />
    {/each}
    <div class="p-2 flex gap-2">
      <button
        class="flex items-center gap-2 p-1 bg-slate-100 rounded text-blue-600 text-sm w-32 justify-center"
        on:click={() => {
          showAddPointer = true;
        }}
      >
        <span class="material-symbols-outlined text-base"> add </span>
        Add Pointer
      </button>
      <button
        class="flex items-center gap-2 p-1 bg-slate-100 rounded text-blue-600 text-sm w-32 justify-center"
      >
        <span class="material-symbols-outlined text-base"> add </span>
        On Hover
      </button>
    </div>
  </div>
{/if}

{#if showAddPointer}
  <Layer
    on:close={() => {
      showAddPointer = false;
    }}
  >
    <div
      class="bg-slate-100 p-4 w-fit h-fit absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
    >
      <form on:submit|preventDefault={createPointer}>
        <h1 class="font-extrabold text-xl mb-2">Pointer Name</h1>
        <p class="mb-2 w-96 font-normal text-xs">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla auctor.
          Sit amet, consectetur adipiscing consectetur adipiscing elit.
        </p>
        <div class="border p-2 border-gray-200 w-96 bg-white rounded relative">
          <p class="text-gray-400 font-bold text-xs mb-1">Name</p>
          <input
            placeholder="Name Your Plane"
            class="text-sm outline-none border-none"
            bind:value={newPointName}
          />
        </div>
        <div class="flex justify-end gap-2 mt-4">
          <button
            class="flex items-center gap-2 p-2 rounded border border-red-600 text-red-600 text-sm w-24 justify-center"
            on:click={() => {
              showAddPointer = false;
            }}
          >
            Cancel
          </button>
          <button
            class="flex items-center gap-2 p-2 ml-2 bg-blue-600 rounded text-white text-sm w-24 justify-center"
            type="submit"
          >
            Create
          </button>
        </div>
      </form>
    </div>
  </Layer>
{/if}
