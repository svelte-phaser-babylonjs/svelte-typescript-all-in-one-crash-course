<script lang="ts">
    import Konva from "konva";
    import {
        createEventDispatcher,
        getContext,
        onDestroy,
        onMount,
        setContext,
    } from "svelte";
    import { layerKey, stageKey } from ".";
    import type { Stage as KonvaStage } from "konva/lib/Stage";

    // export let draggable: boolean = undefined;

    const { getStage } = getContext(stageKey);

    const stage = getStage() as KonvaStage;

    const layer = new Konva.Layer($$props);

    const dispatch = createEventDispatcher();
    layer.on("dragmove click", (e) => {
        dispatch(e.type, e);
    });

    $: layer.setAttrs($$props);

    onDestroy(() => {
        layer.destroy();
    });

    setContext(layerKey, layer);

    stage.add(layer);
</script>

<slot />
