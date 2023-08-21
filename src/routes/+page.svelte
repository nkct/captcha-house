<script lang="ts">
	import Captcha from "./Captcha.svelte";

    let captchas: number[] = [];
</script>

<h1>CAPTCHA</h1>
<button on:click={() => { captchas = [...captchas, 5] }}>Get CAPTCHA</button>
<div class="captcha_container">
    {#each captchas as length}
      <Captcha {length} />
    {/each}
</div>

<style lang="scss">
    .captcha_container {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
</style>