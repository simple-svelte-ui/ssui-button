
<script context="module">
    export const PrimaryState = {
        Visible: true,
        Faded: false,
    };
    export const SecondaryState = {
        Opening: 0,
        Opened: 1,
        Fading: 2,
        Faded: 3,
    };
</script>
<script>
    import { tweened } from "svelte/motion";
    import { quadOut } from "svelte/easing";
    export let color = "#111";
    export let primaryDuration = 250;
    export let secondaryDuration = 250;
    let primaryOpacity;
    setPrimaryOpacityImmediate(0);
    function setPrimaryOpacity(to) {
        primaryOpacity.set(to);
    }
    function setPrimaryOpacityImmediate(to) {
        primaryOpacity = tweened(to, {
            duration: primaryDuration,
            easing: quadOut,
        });
    }
    let secondaryState = SecondaryState.Faded;
    let secondarySplashTimeout;
    let secondaryRippleTimeout;
    let secondaryRippleInterval;
    let secondaryRadius;
    setSecondaryRadiusImmediate(0);
    function setSecondaryRadius(to) {
        secondaryRadius.set(to);
    }
    function setSecondaryRadiusImmediate(to) {
        secondaryRadius = tweened(to, {
            duration: secondaryDuration,
            easing: quadOut,
        });
    }
    let secondaryOpacity;
    setSecondaryOpacityImmediate(0.2);
    function setSecondaryOpacity(to) {
        secondaryOpacity.set(to);
    }
    function setSecondaryOpacityImmediate(to) {
        secondaryOpacity = tweened(to, {
            duration: secondaryDuration,
            easing: quadOut,
        });
    }
    export function drip() {
        setPrimaryOpacity(0.2);
    }
    export function drop() {
        setPrimaryOpacity(0);
    }
    export function splash() {
        switch (secondaryState) {
            case SecondaryState.Opening:
            case SecondaryState.Opened:
            case SecondaryState.Fading:
                if (secondarySplashTimeout) {
                    clearTimeout(secondarySplashTimeout);
                    secondarySplashTimeout = null;
                }
                if (secondaryRippleTimeout) {
                    clearTimeout(secondaryRippleTimeout);
                    secondaryRippleTimeout = null;
                }
                if (secondaryRippleInterval != null) {
                    clearInterval(secondaryRippleInterval);
                    secondaryRippleInterval = null;
                }
                setSecondaryRadiusImmediate(0);
                setSecondaryOpacityImmediate(0.2);
            case SecondaryState.Faded:
                secondaryState = SecondaryState.Opening;
                setSecondaryRadius(100);
                secondaryRippleTimeout = setTimeout(() => {
                    secondaryState = SecondaryState.Opened;
                    secondaryRippleTimeout = null;
                }, secondaryDuration);
                break;
            default:
                break;
        }
    }
    export function ripple() {
        switch (secondaryState) {
            case SecondaryState.Opening:
                if (!secondaryRippleInterval) {
                    secondaryRippleInterval = setInterval(
                        () => ripple(),
                        secondaryDuration / 16
                    );
                }
            case SecondaryState.Opened:
                if (secondaryRippleInterval) {
                    clearInterval(secondaryRippleInterval);
                    secondaryRippleInterval = null;
                }
                if (secondarySplashTimeout) {
                    clearTimeout(secondarySplashTimeout);
                    secondarySplashTimeout = null;
                }
                secondaryRippleTimeout = setTimeout(() => {
                    secondaryState = SecondaryState.Fading;
                    setSecondaryOpacity(0);
                    secondaryRippleTimeout = setTimeout(() => {
                        secondaryState = SecondaryState.Faded;
                        setSecondaryRadiusImmediate(0);
                        setSecondaryOpacityImmediate(0.2);
                        secondaryRippleTimeout = null;
                    }, secondaryDuration);
                }, secondaryDuration);
                break;
            case SecondaryState.Fading:
            case SecondaryState.Faded:
            default:
                break;
        }
    }
</script>
<div id="_" style="--c:{color};">
    <div id="primary" style="--o:{$primaryOpacity};" />
    <div
        id="secondary"
        style="--r:{$secondaryRadius};--o:{$secondaryOpacity};"
    />
</div>
<style>
    #_ {
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        position: absolute;
    }

    #_ div {
        top: -50%;
        left: -50%;
        width: 200%;
        height: 200%;
        position: absolute;
    }

    #_ #primary {
        background: var(--c);
        opacity: var(--o);
    }

    #_ #secondary {
        opacity: var(--o);
        background: radial-gradient(
            circle at 50%,
            var(--c) 0%,
            var(--c) calc(var(--r) * 1%),
            transparent calc(var(--r) * 1%)
        );
    }
</style>
