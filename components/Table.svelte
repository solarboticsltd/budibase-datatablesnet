<script>
    import { Engine, functionCreateDatatable, Pagination, RowsPerPage, Search, Sort } from 'svelte-datatables-net';
    import {onMount} from "svelte"
    export let search   
    export let resultPage
    export let pagination
    export let dataProvider
    export let parRowsPerPage
    export let parSortOrder
    export let columns
    export let parSortBy
    let objectDatatable //= functionCreateDatatable({})
    let tableEvent 

    let th = []
    let displayNames = []
    //let data = {}

    /* onMount(()=>{
        console.log('onMount-------------------------------------------------')
        console.log(dataProvider.rows, 'data')
        if(dataProvider.rows){
            console.log('provider')
            columns.forEach(element=>{
                th.push(element.name)
                displayNames.push(element.displayName)
            })

            console.log("carregou", dataProvider.rows ?? [])
            objectDatatable =  functionCreateDatatable({
                parData: dataProvider.rows ?? [],
                parSearchableColumns: ['title', 'description', 'date'],
                parRowsPerPage: '5',
                parSearchString: '',
                parSortBy: 'title',
                parSortOrder: 'ascending'
            });
        }
    }) */
    /* $: if(data){
        console.log(data, 'data de valores')
        objectDatatable =  functionCreateDatatable(data);
    } */
    
   /*  $:{
        th = []
        displayNames = []
        /* if(dataProvider.rows[0]){
            columns.forEach(element=>{
                th.push(element.name)
                displayNames.push(element.displayName)
            })
        } 
        if(dataProvider.rows){
            columns.forEach(element=>{
                th.push(element.name)
                displayNames.push(element.displayName)
            })
            console.log("carregou", dataProvider.rows ?? [])
            objectDatatable =  functionCreateDatatable({
                parData: dataProvider.rows ?? [],
                parSearchableColumns: ['title', 'description', 'date'],
                parRowsPerPage: '5',
                parSearchString: '',
                parSortBy: 'title',
                parSortOrder: 'ascending'
            });
        }
    } */

    onMount(()=>{
        if(dataProvider.rows){
            columns.forEach(element=>{
                th.push(element.name)
                displayNames.push(element.displayName)
            })
            objectDatatable =  functionCreateDatatable({
                parData: dataProvider.rows ?? [],
                parSearchableColumns: ['title', 'description', 'date'],
                parRowsPerPage: '5',
                parSearchString: '',
                parSortBy: 'title',
                parSortOrder: 'ascending'
            });
        }
    })

    function handleClick(content){
        console.log(content, 'handle clickkkkkkkkkkk')
        tableEvent(JSON.parse(JSON.stringify(content)))
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
    <p>
        {#if search}
            <Search bind:propDatatable={objectDatatable}   propPlaceholder="Type here.. j." />
        {/if}
    </p>
    {#if resultPage}
        <p>
            <RowsPerPage bind:propDatatable={objectDatatable}>
                <option value="5">5</option>
                <option value="10">10</option>
                <option value="20">20</option>
                <option value="30">30</option>
                <option value="all">ALL</option>
            </RowsPerPage>
            <span>RESULTS PER PAGE</span>
        </p>
    {/if}
    
        <table class="table table-striped"  >
            <thead>
                <tr  >
                    {#each displayNames as item}
                        <th>{item}</th>
                    {/each}
                </tr>
            </thead>
            <tbody>
                {#each objectDatatable.arrayData as row}              
                    <tr on:onclick={handleClick}>
                        {#each  th as thItem}
                            <td>
                                {row[thItem]}
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