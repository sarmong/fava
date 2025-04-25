<script lang="ts">
  import { router, set_query_param } from "../router.ts";
  import { filter_params } from "../stores/filters.ts";
  interface Props {
    value: string;
    label: string;
  }

  let { value, label }: Props = $props();

  let isActive = $derived($filter_params.time === value);

  function setTimeFilter() {
    const url = new URL(router.current);
    if (isActive) {
      set_query_param(url, "time", "");
    } else {
      set_query_param(url, "time", value);
    }
    router.navigate(url);
  }
</script>

<a
  href="#time"
  onclick={setTimeFilter}
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
