<script lang="ts" context="module">
    let totalButtons = 0;

    export function getTotalButtons() {
        return totalButtons;
    }
</script>

<script lang="ts">
    import { onDestroy, onMount } from "svelte";

    export let size = "small";
    export let shadow = false;
    export let bgColor = "inherit";
    export let textColor = "inherit";

    let isLeftHovered: boolean;

    let isMount = false;

    onMount(() => {
        totalButtons += 1;
        isMount = true;
    });

    onDestroy(() => {
        totalButtons -= 1;
    });
</script>

{#if isMount}
    {totalButtons}

    <button
        on:click
        style:--buttonBgColor={bgColor}
        style:--buttonTextColor={textColor}
        class:size-lg={size === "large"}
        class:size-sm={size === "small"}
        class:has-left={$$slots.leftContent}
        class:shadow
        {...$$restProps}
    >
        {#if $$slots.leftContent}
            <div
                class="left-content"
                on:mouseenter={() => (isLeftHovered = true)}
                on:mouseleave={() => (isLeftHovered = false)}
                role="button"
                tabindex="0"
            >
                <slot name="leftContent" />
            </div>
        {/if}
        <slot {isLeftHovered}>Fallback</slot>
    </button>
{/if}

<style lang="scss">
    button {
        display: flex;
        align-items: center;
        border: none;
        background-color: var(--buttonBgColor);
        color: var(--buttonTextColor);
        font-weight: bold;
        border-radius: 5px;
        cursor: pointer;
        .left-content {
            margin-right: 10px;
        }
        &:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }
        &:hover {
            background-image: linear-gradient(rgba(0, 0, 0, 0.4) 0 0);
        }
        &:active {
            background-image: linear-gradient(rgba(255, 255, 255, 0.1) 0 0);
        }
        &.size-sm {
            padding: 15px 20px;
        }
        &.size-lg {
            padding: 20px 25px;
            font-size: 20px;
        }
        &.shadow {
            box-shadow: 0 0 10px rgba(1, 1, 1, 0.3);
        }
    }
</style>
