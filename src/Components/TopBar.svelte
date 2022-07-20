<script>
  import Layer from "./Layer.svelte";
  import { createEventDispatcher } from "svelte";
  import { v4 as uuidv4 } from "uuid";
  import MemberAdditionBar from "./MemberAdditionBar.svelte";
  export let showAccess = false;
  let showAddPlan = false;
  let newPlaneName = "";
  export let plans;
  const dispatch = createEventDispatcher();

  const createPlan = () => {
    if (newPlaneName === "") {
      return;
    }
    plans = [
      ...plans,
      {
        id: uuidv4(),
        title: newPlaneName,
        open: false,
        subcategories: [],
        teammates: [],
      },
    ];
    showAddPlan = false;
    newPlaneName = "";
    dispatch("change", plans);
  };
</script>

<div class="flex">
  <h1 class="text-2xl font-extrabold">Action Plans</h1>
  <div class="ml-auto flex">
    <button
      class="flex items-center gap-2 p-2 rounded border border-blue-600 text-blue-600 text-sm"
      on:click={() => {
        showAccess = true;
      }}
    >
      <span class="material-symbols-outlined text-base"> group </span>
      Manage Access
    </button>
    <button
      class="flex items-center gap-2 p-2 ml-2 bg-blue-600 rounded text-white text-sm w-32 justify-center"
      on:click={() => {
        showAddPlan = true;
      }}
    >
      <span class="material-symbols-outlined rounded bg-blue-500"> add </span>
      New Plan
    </button>
  </div>
</div>
{#if showAccess}
  <Layer
    on:close={() => {
      showAccess = false;
    }}
  >
    <div
      class="bg-slate-100 p-4 w-fit h-fit absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
    >
      <h1 class="font-extrabold text-xl mb-2">SOP Access</h1>
      {#each plans as plan (plan.id)}
        <div class="my-2">
          <h2 class="text-base font-bold text-blue-600">{plan.title}</h2>
          <MemberAdditionBar
            teammates={plan.teammates}
            on:add={(e) => {
              plans = plans.map((p) => {
                if (plan.id === p.id) {
                  return {
                    ...p,
                    teammates: [...p.teammates, e.detail],
                  };
                }
                return p;
              });
              dispatch("change", plans);
            }}
            on:delete={(e) => {
              plans = plans.map((p) => {
                if (plan.id === p.id) {
                  return {
                    ...p,
                    teammates: [...p.teammates.filter((id) => id !== e.detail)],
                  };
                }
                return p;
              });
              dispatch("change", plans);
            }}
          />
        </div>
      {/each}
    </div>
  </Layer>
{/if}
{#if showAddPlan}
  <Layer
    on:close={() => {
      showAddPlan = false;
    }}
  >
    <div
      class="bg-slate-100 p-4 w-fit h-fit absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
    >
      <form on:submit|preventDefault={createPlan}>
        <h1 class="font-extrabold text-xl mb-2">Plan Name</h1>
        <p class="mb-2 w-96 font-normal text-xs">
          Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla auctor.
          Sit amet, consectetur adipiscing consectetur adipiscing elit.
        </p>
        <div class="border p-2 border-gray-200 w-96 bg-white rounded relative">
          <p class="text-gray-400 font-bold text-xs mb-1">Name</p>
          <input
            placeholder="Name Your Plane"
            class="text-sm outline-none border-none"
            bind:value={newPlaneName}
          />
        </div>
        <div class="flex justify-end gap-2 mt-4">
          <button
            class="flex items-center gap-2 p-2 rounded border border-red-600 text-red-600 text-sm w-24 justify-center"
            on:click={() => {
              showAddPlan = false;
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
