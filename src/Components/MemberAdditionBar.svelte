<script>
  export let teammates = [];
  import { createEventDispatcher } from "svelte";
  import peoples from "../peoples.json";
  $: teammatesDetails = teammates.map((id) =>
    peoples.find((people) => people.id === id)
  );
  const dispatch = createEventDispatcher();
  let showDropDown = false;
</script>

<div class="border p-2 border-gray-200 w-96 bg-white rounded relative">
  <p class="text-gray-400 font-bold text-xs mb-1">TEAMMATES</p>
  {#if teammatesDetails.length !== 0}
    <div class="flex gap-2">
      {#each teammatesDetails as detail}
        <div class="rounded flex gap-2 items-center bg-blue-100 p-1 px-2">
          <img alt={detail.name} src={detail.img} class="w-3 h-3" />
          <p class="text-xs font-black">{detail.name}</p>
        </div>
      {/each}
    </div>
  {:else}
    <p class="text-sm">Select Members</p>
  {/if}
  <div
    class="absolute top-1/2 right-2 -translate-y-1/2 cursor-pointer"
    on:click={() => {
      showDropDown = !showDropDown;
    }}
  >
    <span
      class="material-symbols-outlined rounded bg-gray-400 text-gray-300 text-base w-5 h-5 flex items-center justify-center"
    >
      add
    </span>
  </div>
  {#if showDropDown}
    <div class="absolute top-full left-0 w-full p-2 bg-white z-50">
      {#each peoples as people}
        <div
          class="mb-2 flex gap-2 items-center border-b border-gray-100 p-1 cursor-pointer hover:bg-blue-100"
          on:click={() => {
            if (teammates.includes(people.id)) {
              dispatch("delete", people.id);
            } else {
              dispatch("add", people.id);
            }
            showDropDown = false;
          }}
        >
          <img alt={people.name} src={people.img} class="w-3 h-3" />
          <p class="text-sm font-black">{people.name}</p>
          {#if teammates.includes(people.id)}
            <span
              class="material-symbols-outlined ml-auto rounded bg-red-400 text-red-300 text-base w-4 h-4 flex items-center justify-center"
            >
              close
            </span>
          {/if}
        </div>
      {/each}
    </div>
  {/if}
</div>
