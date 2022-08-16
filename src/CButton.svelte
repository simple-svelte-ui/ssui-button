<script>
    import { createEventDispatcher } from "svelte";
    import Ripple from "./Ripple.svelte";
    export let variant = "text";
    export let backgroundColor = "#2476d2";
    export let foregroundColor = "#fff";
    export let rippleColor = "#fff";
    export let textTransform = "uppercase";
    export let fontWeight = "500";
    let ripple;
    let button;
    const dispatcher = createEventDispatcher();

    function onFocus(event) {
        ripple.drip();
        dispatcher("focus", event);
    }

    function onBlur(event) {
        ripple.drop();
        dispatcher("blur", event);
    }

    function onMouseDown(event) {
        ripple.splash();
        window.addEventListener("mouseup", onMouseUp);
        dispatcher("mousedown", event);
    }

    function onMouseUp(event) {
        ripple.ripple();
        window.removeEventListener("mouseup", onMouseUp);
        dispatcher("mouseup", event);
    }
</script>

<button
    id="_"
    class={variant}
    style="--rc:{rippleColor};--fc:{foregroundColor};--bc:{backgroundColor};--tt:{textTransform};--fw:{fontWeight}"
    bind:this={button}
    on:click
    on:mouseover
    on:mouseout
    on:focus={onFocus}
    on:blur={onBlur}
    on:mousedown={onMouseDown}
    on:mouseup={onMouseUp}
>
    <Ripple
        bind:this={ripple}
        color={(() => {
            switch (variant) {
                case "contained":
                    return rippleColor;
                case "outlined":
                case "text":
                    return backgroundColor;
                case "disabled":
                    return "transparent";
                default:
                    break;
            }
        })()}
    />
    <slot />
</button>

<!-- Styles -->
<style>
    #_ {
        all: unset;
        display: inline-flex;
        justify-content: center;
        align-items: center;
        min-width: 2em;
        height: 2em;
        padding: 0 16px 0 16px;
        border-radius: 4px;
        overflow: hidden;
        position: relative;
        font-weight: var(--fw);
        text-transform: var(--tt);
        color: var(--fc);
        background: var(--bc);
        cursor: pointer;
        user-select: none;
    }

    #_.outlined {
        color: var(--bc);
        background: transparent;
        border: 2px var(--bc) solid;
    }

    #_.text {
        color: var(--bc);
        background: transparent;
    }
</style>
