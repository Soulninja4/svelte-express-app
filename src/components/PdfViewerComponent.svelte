<script>
    import { afterUpdate, onMount, onDestroy, beforeUpdate } from 'svelte';
    import PSPDFKit from 'pspdfkit'

    let currentDocument
    let container
    let instance

    export let document

    async function load() {
        currentDocument = document
        instance = await PSPDFKit.load({
            baseUrl: `${window.location.protocol}//${window.location.host}/`,
            container: container,
            document: document
        })
    }

    function unload() {
        PSPDFKit.unload(instance)
        instance = null
    }

    onMount(() => {
        load()
    })

    afterUpdate(() => {
        if (document !== currentDocument) {
            unload()
            load()
        }
    })

    onDestroy(() => {
        unload()
    })

</script>
<body>
    <div bind:this={container} style="height: 80vh; width: 45vw;"></div>
</body>