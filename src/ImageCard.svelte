<script>



    export let data;


    function OnOpen(url) {
        window.open(url, '_blank').focus();
    }

    let temp=data.tags.split(" ")
    let tags=[]
    temp.forEach(item=>{
        tags=[...item.split(',')]
    })

    console.log(tags)


</script>


<div class="container">

    <div style="position: relative;">
        <img id="i" src={data.image_url} alt="Avatar" class="image" style="width:100%">
        <div class="middle">
            <div  on:click={OnOpen(data.canonical_url)} class="text">{data.domain} <svg style="width:18px;height:18px" viewBox="0 0 24 24">
                <path fill="currentColor" d="M8.5,18.31L5.69,15.5L12.06,9.12H7.11V5.69H18.31V16.89H14.89V11.94L8.5,18.31Z" />
            </svg></div>
        </div>
    </div>

    <div style="display: flex;margin-top: 12px">
        {#each tags.slice(0,4) as tag}
            <p style="background-color: #eee;border-radius: 12px;padding: 6px 12px;margin-right: 4px">{tag}</p>
        {/each}
    </div>

    <div style="font-weight: bold">{data.name}</div>
<!--    <div style="font-size: 12px">{data.description.length>20?`${data.description.slice(0,40)}...`:data.description}</div>-->


</div>

<style>


    .container {

        width: 270px;
        padding: 16px;
        margin: 8px 4px;
    }
    .image{
        border-radius: 8px;
    }

    .image {
        opacity: 1;
        display: block;
        width: 100%;
        height: auto;
        transition: .5s ease;
        backface-visibility: hidden;
    }

    .middle {
        transition: .5s ease;
        opacity: 0;
        position: absolute;
        bottom: 20px;
        left: 20px;
        /*transform: translate(-50%, -50%);*/
        /*-ms-transform: translate(-50%, -50%);*/
        text-align: center;
    }

    .container:hover .image {
        opacity: 0.3;
    }

    .container:hover .middle {
        opacity: 1;
    }

    .text {
        display: flex;
        align-content: center;
        background-color: transparent;
        color: #111111;
        font-size: 16px;
        font-weight: bold;
        cursor: pointer;
    }




</style>