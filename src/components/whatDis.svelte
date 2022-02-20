<script>
    export let close;
</script>

<main>

<section>
    <h1>What dis?</h1>
    <a on:click={close}>Close</a>
</section>

</main>

<style lang="scss">
    main {
        position: absolute;
        width: 100%;
        height: 100%;
        background: #00000088;
        top: 0;
        left: 0;
    }
    section {
        position: absolute;
        min-width: 30em;
        min-height: 30em;

        background: hsl(0, 0%, 8%);
        border: 2px solid hsl(0, 0%, 13%);
        padding: 1em;

        border-radius: 0.7em;

        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }

    h1 {
        margin: 0;
    }
</style>