<script lang="ts">
	import { onMount } from "svelte";
    
    export let length = 5;
    export let with_noise = true;
    export let with_bars = true;

    let captcha_string = "";

    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    const canvas_width = 250;
    const canvas_height = 100;

    for (let i = 0; i < length; i++) {
        captcha_string += characters.charAt(Math.floor(Math.random() * characters.length));
    }

    let captcha: HTMLDivElement;
    let canvas: HTMLCanvasElement;
    let input: HTMLInputElement;

    function random_transform() {
        return {
            a: 1,
            b: (Math.random() * 0.5) - 0.25,
            c: (Math.random() * 2) - 1,
            d: 1,
            e: 0,
            f: 0,
        }
    }

    function add_noise(ctx: CanvasRenderingContext2D, src:ImageData): ImageData {
        let dst = new ImageData(src.data, src.width, src.height);
        let mangled = ctx.createImageData(canvas_width, canvas_height);
        for (let i = 0; i < dst.data.length; i++) {
            const px = dst.data[i];
            mangled.data[i] = px + (Math.random() * 200)
        }
        return mangled
    }

    function add_bars(ctx: CanvasRenderingContext2D, src:ImageData): ImageData {
        let dst = new ImageData(src.data, src.width, src.height);
        let x = 0;
        while (x < 25) {
            let step = Math.floor(Math.random() * dst.width * 4);
            let row = 0;
            while (row <= src.height) {
                let n = step + (src.width * row * 4) - 1
                dst.data[n] = 255
                dst.data[n - 4] = 255
                
                row += 1;
            }
            x += 1;
        }
        
        return dst
    }

    onMount(() => {
        const ctx = canvas.getContext('2d');
        if (ctx != null) {
            ctx.font = "30px arial bold";
            let {a, b, c, d, e, f} = random_transform();
            ctx.transform(a, b, c, d, e, f);
            ctx.filter = `blur(${Math.random() * 2 - 1}px)`
            ctx.fillText(
                captcha_string, 
                (canvas_width / 2) - (ctx.measureText(captcha_string).width / 2), 
                canvas_height / 2 + 15);

            let src = ctx.getImageData(0, 0, canvas_width, canvas_height);
            if (with_noise) {
                src = add_noise(ctx, src);
            }
            if (with_bars) {
                src = add_bars(ctx, src);
            }
            ctx.putImageData(src, 0, 0);
        }
    })
</script>

<div class="text-captcha" bind:this={captcha}>
    <canvas bind:this={canvas} width={canvas_width} height={canvas_height}></canvas>
    <form>
        <input type="text" placeholder="Input the above text here..." bind:this={input} />
        <input type="submit" value="Submit" on:click={() => { 
            if (input.value == captcha_string) {
                captcha.style.border = "5px lightgreen solid"
                setTimeout(() => {
                    captcha.parentNode?.removeChild(captcha)
                }, 500);
            } else {
                captcha.style.border = "5px lightcoral solid"
                input.value = "Correct answer: " + captcha_string;
                setTimeout(() => {
                    captcha.parentNode?.removeChild(captcha);
                }, 3000);
            }
        }} />
    </form>
</div>

<style lang="scss">
    .text-captcha {
        border-radius: 10px;
        border: 5px lightgray solid;
        margin: 7px 0;
    }
</style>
