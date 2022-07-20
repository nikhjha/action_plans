<script>
  import Layer from "./Layer.svelte";
  import { slide } from "svelte/transition";
  import { createEventDispatcher } from "svelte";
  import { v4 as uuidv4 } from "uuid";
  import CategoryBar from "./CategoryBar.svelte";
  export let plan;
  let showDropDown = false;
  let showDeletePlan = false;
  const dispatch = createEventDispatcher();
</script>

<div class="w-full rounded-md bg-slate-100 p-2 px-4 flex gap-2 text-gray-400">
  <span class="material-symbols-outlined cursor-pointer"> drag_indicator </span>
  <h4 class="font-extrabold text-black">{plan.title}</h4>
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
        class="absolute top-full right-0 bg-white z-20 w-32 p-1 border border-gray-300"
      >
        <button
          class="p-1 text-sm "
          on:click={() => {
            showDropDown = false;
          }}
        >
          Edit Category
        </button>
        <button
          class="p-1 text-sm "
          on:click={() => {
            showDropDown = false;
            dispatch("showAccess");
          }}
        >
          Manage Access
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
  {#if plan.open}
    <span
      class="material-symbols-outlined cursor-pointer"
      on:click={() => {
        dispatch("expand", plan.id);
      }}
    >
      expand_more
    </span>
  {:else}
    <span
      class="material-symbols-outlined cursor-pointer"
      on:click={() => {
        dispatch("expand", plan.id);
      }}
    >
      chevron_right
    </span>
  {/if}
  {#if showDeletePlan}
    <Layer
      on:close={() => {
        showDeletePlan = false;
      }}
    >
      <div
        class="bg-slate-100 p-4 w-fit h-fit absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 text-black"
      >
        <h1 class="font-extrabold text-xl mb-2">Delete Category</h1>
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
              dispatch("delete", plan.id);
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
{#if plan.open}
  <div class="flex flex-col gap-2" transition:slide>
    {#each plan.subcategories as category (category.id)}
      <CategoryBar
        {category}
        on:delete={(e) => {
          dispatch("delete_category", { plan: plan.id, category: e.detail });
        }}
        on:expand={(e) => {
          dispatch("expand_category", { plan: plan.id, category: e.detail });
        }}
        on:delete_pointer={(e) => {
          dispatch("delete_pointer", { plan: plan.id, ...e.detail });
        }}
        on:add_pointer={(e) => {
          dispatch("add_pointer", {
            plan: plan.id,
            category: e.detail.category,
            pointer: {
              name: e.detail.pointer,
              id: uuidv4(),
            },
          });
        }}
      />
    {/each}
  </div>
{/if}
