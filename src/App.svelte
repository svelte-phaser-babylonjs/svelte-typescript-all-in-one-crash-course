<script lang="ts">
    import ColorPicker from "./components/ColorPicker.svelte";
    import Form from "./components/Form.svelte";
    import LifeCycle from "./components/LifeCycle.svelte";
    import Quote from "./components/Quote.svelte";
    import GeoLocation from "./components/GeoLocation.svelte";
    import Funny from "./Funny.svelte";
    import Counter from "./components/Counter.svelte";
    import countStore from "./stores/Count.Store";
    import CounterCustom from "./components/CounterCustom.svelte";
    import countCustomStore from "./stores/Custom.Store";
    import Action from "./components/Action.svelte";
    import Draggable from "./components/Draggable.svelte";
    import CustomEventExample from "./components/CustomEventExample.svelte";
    import tippyAction from "./libs/actions/tippyAction";
    import MainForm from "./components/ContextExample/MainForm.svelte";
    import KonvaExample from "./components/KonvaSvelte/KonvaExample.svelte";
    import MCExample from "./components/ModuleContextExample/MCExample.svelte";

    let customName = "James";
    let name = "Navid";
    let disabled = false;

    // create a reactive variable that when name changes it will be updated
    $: capitalizedName = name.toUpperCase();

    let fontSize = 30;

    let controlHold = false;

    setTimeout(() => {
        name = "Navid Dezashibi";
    }, 1000);

    function clickMe(): void {
        alert("clicked");
    }

    function onChange(e: Event) {
        const target = e.target as HTMLInputElement; // Type assertion
        alert(`changed to ${target.value}`);
    }

    function onKeyDown(e: KeyboardEvent) {
        console.log(e.key, e, "keydown");

        if (e.key == "Control") {
            controlHold = true;
        }

        if (controlHold) {
            switch (e.key) {
                case "v":
                    alert("pasted!");
                    break;

                case "c":
                    alert("copied!");
                    break;

                case "x":
                    alert("cut!");
                    break;

                default:
                    break;
            }
        }
    }

    function onKeyUp(e: KeyboardEvent) {
        console.log(e.key, e, "keyup");

        if (e.key == "Control") {
            controlHold = false;
        }
    }

    function onFormSubmit(e: CustomEvent) {
        const { name, age } = e.detail;

        alert(`${name} is ${age} years old!`);
    }

    let colorList = ["red", "green", "blue", "purple"];

    setTimeout(() => {
        colorList = ["Yellow", ...colorList];
    }, 2000);

    let show = false;
</script>

<Funny {name} wait={4000} />
<Funny name={customName} favoriteColorBegin="green" favoriteColorEnd="red" />

<h1 style="font-size: {fontSize}px">Hello Again {name}!</h1>

<input type="text" bind:value={name} />

<span>{capitalizedName}</span>

<button {disabled} on:click={clickMe}>Click Me</button>

<input
    type="range"
    bind:value={fontSize}
    min="10"
    max="50"
    on:change={onChange}
/>
<span>{fontSize}</span>

<svelte:window on:keydown={onKeyDown} on:keyup={onKeyUp} />

<hr />

<ColorPicker />

<hr />

<Form on:form-submit={onFormSubmit} />

<hr />

<ul>
    {#each colorList as color, index}
        {#if color == "green"}
            <li>{index + 1} - {color} 👍</li>
        {:else}
            <li>{index + 1} - {color}</li>
        {/if}
    {/each}
</ul>

<hr />

<Quote />

<hr />
<input type="checkbox" bind:checked={show} />
{#if show}
    <LifeCycle />
    <GeoLocation />
{/if}

<hr />
<Counter />

<h3>{$countStore}</h3>

<CounterCustom />

<h3>{$countCustomStore}</h3>

<hr />

<Action />

<hr />
<br />
<br />
<br />

<Draggable left={800} top={100}>
    <h1>Drag Me!</h1>
</Draggable>

<hr />
<br />
<br />

<CustomEventExample />

<button use:tippyAction={{ content: "from outside" }}>In the main app</button>

<hr />
<br />

<MainForm />

<hr />
<br />

<KonvaExample />

<hr />
<br />

<MCExample />

<style>
    h1 {
        color: red;
    }
</style>
