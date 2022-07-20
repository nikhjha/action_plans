<script>
  import DataBar from "./DataBar.svelte";
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let plans;
</script>

<div class="mt-8 flex flex-col gap-2">
  {#each plans as plan (plan.id)}
    <DataBar
      {plan}
      on:delete={(e) => {
        plans = plans.filter((plan) => plan.id !== e.detail);
        dispatch("change", plans);
      }}
      on:expand={(e) => {
        plans = plans.map((plan) => {
          if (plan.id === e.detail) {
            return {
              ...plan,
              open: !plan.open,
            };
          }
          return plan;
        });
        dispatch("change", plans);
      }}
      on:showAccess={() => {
        dispatch("showAccess");
      }}
      on:delete_category={(e) => {
        plans = plans.map((plan) => {
          if (plan.id === e.detail.plan) {
            return {
              ...plan,
              subcategories: plan.subcategories.filter(
                (category) => category.id !== e.detail.category
              ),
            };
          }
          return plan;
        });
        dispatch("change", plans);
      }}
      on:expand_category={(e) => {
        plans = plans.map((plan) => {
          if (plan.id === e.detail.plan) {
            return {
              ...plan,
              subcategories: plan.subcategories.map((category) => {
                if (category.id === e.detail.category) {
                  return {
                    ...category,
                    open: !category.open,
                  };
                }
                return category;
              }),
            };
          }
          return plan;
        });
        dispatch("change", plans);
      }}
      on:add_pointer={(e) => {
        plans = plans.map((plan) => {
          if (plan.id === e.detail.plan) {
            return {
              ...plan,
              subcategories: plan.subcategories.map((category) => {
                if (category.id === e.detail.category) {
                  return {
                    ...category,
                    pointers: [...category.pointers, e.detail.pointer],
                  };
                }
                return category;
              }),
            };
          }
          return plan;
        });
        dispatch("change", plans);
      }}
      on:delete_pointer={(e) => {
        plans = plans.map((plan) => {
          if (plan.id === e.detail.plan) {
            return {
              ...plan,
              subcategories: plan.subcategories.map((category) => {
                if (category.id === e.detail.category) {
                  return {
                    ...category,
                    pointers: category.pointers.filter(
                      (pointer) => pointer.id !== e.detail.pointer
                    ),
                  };
                }
                return category;
              }),
            };
          }
          return plan;
        });
        dispatch("change", plans);
      }}
    />
  {/each}
</div>
