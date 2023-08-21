<script lang="ts">
	import Captcha from "./Captcha.svelte";

    let captchas: number[] = [];
</script>

<h1>CAPTCHA</h1>
<button on:click={() => { captchas = [...captchas, 5] }}>Get CAPTCHA</button>
{#each captchas as length}
      <Captcha {length} />
{/each}