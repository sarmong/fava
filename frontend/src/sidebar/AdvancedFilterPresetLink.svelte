<script lang="ts">
  import { router, set_query_param } from "../router.ts";
  import { filter_params } from "../stores/filters.ts";

  interface Props {
    value: string;
    label: string;
  }

  let { value, label }: Props = $props();

  // Track if the filter is active (present in the current filter as a substring)
  let isActive = $derived($filter_params.filter.includes(value));

  function setAdvancedFilter() {
    const currentFilter = $filter_params.filter;

    const url = new URL(router.current);
    // Check if the value is already a substring of the current filter
    if (currentFilter.includes(value)) {
      // If it's already in the filter, remove it
      const newFilter = currentFilter.replace(value, "").trim();

      set_query_param(url, "filter", newFilter);
    } else {
      // If it's not in the filter, append it
      // If the current filter is empty, just set the value
      if (!currentFilter) {
        set_query_param(url, "filter", value);
      } else {
        // Otherwise, append space-separated value
        set_query_param(url, "filter", `${currentFilter} ${value}`);
      }
    }
    router.navigate(url);
  }
</script>

<a
  href="#adv"
  onclick={setAdvancedFilter}
  class={{ "filter-preset": true, active: isActive }}>{label}</a
>

<style>
  a:link.filter-preset,
  a:visited.filter-preset {
    color: #66c4ff;
  }

  a:link:hover.filter-preset {
    color: #737373;
  }

  a.active.filter-preset {
    font-weight: bold;
    text-decoration: underline;
  }
</style>
