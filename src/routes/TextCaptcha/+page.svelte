<script lang="ts">
	import TextCaptcha from "./TextCaptcha.svelte";
	import Sidebar from "../Sidebar.svelte";

    let text_captchas: {length: number, with_noise: boolean, with_bars: boolean}[] = [];
</script>

<Sidebar />

<div class="content">
    <h1>CAPTCHA</h1>
    <form class="text-captcha-form" on:submit={(e) => {
        let target = e.target;
        if (target != null && target instanceof HTMLFormElement) {
            let form_data = Object.fromEntries(new FormData(target));
            let data = {
                length: Number(form_data.length), 
                with_noise: false, 
                with_bars: false
            }
            if (form_data.hasOwnProperty("with_noise")) {
                data.with_noise = true
            }
            if (form_data.hasOwnProperty("with_bars")) {
                data.with_bars = true
            }
    
            text_captchas = [...text_captchas, data]
        }
    }}>
        <input type="number" name="length" value="5" style="width: 50px;"/>
        <label for="length">Length</label>
    
        <input type="checkbox" name="with_noise" />
        <label for="with_noise">With noise</label>
    
        <input type="checkbox" name="with_bars" />
        <label for="with_bars">With bars</label>

        <br>
    
        <input type="submit" value="Get CAPTCHA">
    </form>
    <div class="captcha_container">
        {#each text_captchas as props}
          <TextCaptcha length = {props.length} with_noise = {props.with_noise} with_bars = {props.with_bars} />
        {/each}
    </div>
</div>

<style lang="scss">
    .content {
        text-align: center;
    }
    .text-captcha-form {
        display: block;
    }
    .captcha_container {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
</style>