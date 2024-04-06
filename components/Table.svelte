<script>
  import {Engine, functionCreateDatatable, Pagination, RowsPerPage, Search, Sort} from "svelte-datatables-net";
  import { onMount } from "svelte";

  export let search;
  export let resultPage;
  export let pagination;
  export let dataProvider;
  export let columns;
  export let searchableColumns;
  export let rowsPerPage;
  export let sortOrder;
  export let sortBy;
  export let tableEvent;

  let objectDatatable;

  let tableHeader = [];
  let displayNames = [];

  onMount(() => {
    if (dataProvider.rows) {
      columns.forEach((element) => {
        tableHeader.push(element.name);
        displayNames.push(element.displayName);
      });
      objectDatatable = functionCreateDatatable({
        parData: dataProvider.rows ?? [],
        parSearchableColumns: searchableColumns,
        parRowsPerPage: rowsPerPage.toString(),
        parSearchString: "",
        parSortBy: sortBy,
        parSortOrder: sortOrder,
      });
    }
  });

  function handleClick(content) {
    tableEvent({ data: JSON.parse(JSON.stringify(content)) });
  }
</script>

<svelte:head>
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css"
    rel="stylesheet"
    integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC"
    crossorigin="anonymous"
  />
</svelte:head>

{#if objectDatatable}
  <Engine bind:propDatatable={objectDatatable} />

  <div class="card">
    <div class="d-flex justify-content-between p-3">
      <p>
        {#if search}
          <Search class="form-control" bind:propDatatable={objectDatatable} propPlaceholder="Search..." />
        {/if}
      </p>
      {#if resultPage}
        <p>
          <RowsPerPage class="form-control" bind:propDatatable={objectDatatable} >
            <option value="5">5</option>
            <option value="10">10</option>
            <option value="20">20</option>
            <option value="30">30</option>
            <option value="all">ALL</option>
          </RowsPerPage>
          <span>Results per page</span>
        </p>
      {/if}
    </div>

    <table class="table table-striped">
      <thead>
        <tr>
          {#each displayNames as item}
            <th>
                <Sort bind:propDatatable={objectDatatable} propColumn={item}>
                    {item}
                </Sort>
            </th>
          {/each}
        </tr>
      </thead>
      <tbody>
        {#each objectDatatable.arrayData as row}
          <tr on:click={handleClick(row)}>
            {#each tableHeader as tableHeaderItem}
              <td>
                {row[tableHeaderItem]}
              </td>
            {/each}
          </tr>
        {/each}
      </tbody>
    </table>
    {#if pagination}
      <p>
        <Pagination bind:propDatatable={objectDatatable} propSize="small" />
      </p>
    {/if}
  </div>
{/if}
