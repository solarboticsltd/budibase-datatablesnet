<script>
  import { getContext } from "svelte"
  import Table from "../components/Table.svelte";

  export let search
  export let resultPage
  export let pagination
  export let dataProvider
  export let columns
  export let searchableColumns
  export let rowsPerPage
  export let sortOrder
  export let sortBy
  export let tableEvent

  const { styleable } = getContext("sdk")
  const component = getContext("component")
</script>

<div use:styleable={$component.styles}>
  <Table dataProvider={dataProvider}
    search={search}
    resultPage={resultPage}
    pagination={pagination}
    columns={columns}
    searchableColumns={searchableColumns}
    rowsPerPage={rowsPerPage}
    sortOrder={sortOrder}
    sortBy={sortBy}
    tableEvent={tableEvent} />
</div>
