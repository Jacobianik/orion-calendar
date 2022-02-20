<script>
    import WhatDis from "./whatDis.svelte"

    let popupOpen = false
</script>

<footer>
    <span>Made by <a href="//jacobhaap.com" target="_blank">Jacob Haap</a> and <a href="//ant.lgbt" target="_blank">Yanik Ammann</a></span>
    <span>Find out what this is: <a on:click={() => popupOpen = true}>here</a></span>
</footer>

{#if popupOpen}
    <WhatDis close={() => popupOpen = false}/>
{/if}

<style>
    footer {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }

    span {
        margin: 0.15em;
    }
</style>