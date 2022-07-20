<script>
  import DataPanel from "./Components/DataPanel.svelte";
  import TopBar from "./Components/TopBar.svelte";
  import defaultPlans from "./plans.json";
  let plans = defaultPlans;
  const change = (e) => {
    plans = e.detail;
  };
  let showAccess = false;
</script>

<main>
  <div class="container mx-auto max-w-4xl w-full p-2 my-5">
    <TopBar {plans} on:change={change} {showAccess} />
    <DataPanel
      {plans}
      on:change={change}
      on:showAccess={() => {
        showAccess = false;
        showAccess = true;
      }}
    />
  </div>
</main>
