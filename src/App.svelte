<script>
	import {onMount} from "svelte";
	import InfiniteScroll from "./InfiniteScroll.svelte";
    import ImageCard from "./ImageCard.svelte";

	// if the api (like in this example) just have a simple numeric pagination
    let page = 0;
    // but most likely, you'll have to store a token to fetch the next page
    let nextUrl = '';
    // store all the data here.
    let data = [];
    // store the new batch of data here.
    let newBatch = [];
    let searchTerm = ""
    let filteredArr = []

    async function fetchData() {
        const response = await fetch(`http://127.0.0.1:3010/?offset=${page * 60}`, {
            headers: {"Content-type": "application/json;charset=UTF-8", 'Access-Control-Allow-Origin': '*'}
        });
        newBatch = await response.json();
        console.log(newBatch)
    };

    onMount(() => {
        // load first batch onMount
        fetchData();
    })

    $: data = [
        ...data,
        ...newBatch
    ];

    $:{
        if (searchTerm === "") {
            filteredArr = [...data]
        } else {
            filteredArr = data.filter(item => item.name.includes(searchTerm))
        }
    }


</script>
<style>
    body{
        background-color: #f6f6f6;

    }

    main {
        display: flex;
        width: 100%;
        height: 100%;
        align-items: center;
        justify-content: center;
        flex-direction: column;
    }

    .images {

        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
        height: 100%;
        overflow-x: scroll;
        list-style: none;
        padding: 0;
		background-color: transparent;

	}

    input{
        border-radius: 3px;
        width: 650px;
        height: 48px;
        padding: 24px
    }

    @media only screen and (max-width: 600px) {
        input {
            width: 250px;
            height: 48px;
        }
    }



</style>

<main>
    <input bind:value={searchTerm} placeholder="Search...">

    <div class="images" >
        {#each filteredArr as item}
            <ImageCard  data={item}/>
        {/each}
        <InfiniteScroll
                hasMore={newBatch.length}
                threshold={100}
                on:loadMore={() => {page++; fetchData()}}/>
    </div>

</main>