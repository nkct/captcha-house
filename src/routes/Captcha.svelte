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
        if (ctx != null) {
            ctx.font = "30px arial"
            ctx.fillText(captcha_string, canvas_width/2 - 30, canvas_height/2 + 15);
        }
    })

    console.log(captcha_string);
</script>

<div class="captcha">
    <canvas bind:this={canvas} width={canvas_width} height={canvas_height}></canvas>
    <form>
        <input type="text" placeholder="Input the above text here..." bind:this={input} />
        <input type="submit" value="Submit" on:click={() => { console.log(input.value == captcha_string) }} />
    </form>
</div>

<style lang="scss">
    .captcha {
        border-radius: 10px;
        border: 5px lightgray solid;
        margin: 7px 0;
    }
</style>
