<script lang="ts">
	import { onMount } from "svelte";

    export let length = 5;
    let captcha_string = "";

    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    const canvas_width = 200;
    const canvas_height = 100;

    for (let i = 0; i < length; i++) {
        captcha_string += characters.charAt(Math.floor(Math.random() * characters.length));
    }

    let canvas: HTMLCanvasElement;
    let input: HTMLInputElement;

    onMount(() => {
        const ctx = canvas.getContext('2d');
        ctx?.fillText(captcha_string, 10, 10);
    })

    console.log(captcha_string);
</script>

<canvas bind:this={canvas} width={canvas_width} height={canvas_height}></canvas>
<form>
    <input bind:this={input} type="text" />
    <input type="submit" on:click={() => { console.log(input.value == captcha_string) }} />
</form>