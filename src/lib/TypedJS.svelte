<script lang="ts">
    import { onMount } from "svelte";
    import Typed from "typed.js";

    export let initialString:string
    export let strings: string[]

    let el:HTMLSpanElement

    onMount(() => {
        const typed:Typed = new Typed(el, {
            strings: strings,
            typeSpeed: 50,
            backSpeed: 20,
            backDelay: 900,
            startDelay: 0,
            loop: false,
            onLastStringBackspaced: () => typed.strPos = 0,
            onComplete: (self) => { self.cursor.remove() }
        })
    })
</script>

<span bind:this={el}>{initialString}</span>