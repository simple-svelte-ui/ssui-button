<script>
    import CButton from "./CButton.svelte";
    let variant = "text";
    let color = "primary";
    const colors = {
        primary: { color: "#2476d2", rippleColor: "#9cc2e8" },
        secondary: { color: "#6200ee", rippleColor: "#bf7fcf" },
        success: { color: "#467d32", rippleColor: "#82d365" },
        error: { color: "#b70000", rippleColor: "#f0a3a3" },
        warning: { color: "#caad1d", rippleColor: "#eec7a0" },
        disabled: { color: "#e0e0e0", rippleColor: "#fff" },
    };
    export { variant, color };
</script>

<CButton
    on:blur
    on:click
    on:focus
    on:mousedown
    on:mouseup
    on:mouseout
    on:mouseover
    {variant}
    backgroundColor={colors[color].color}
    foregroundColor="#fff"
    rippleColor={colors[color].rippleColor}><slot /></CButton
>
