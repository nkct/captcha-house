<script lang="ts">
	import { onMount } from "svelte";

    let sidebar: HTMLDivElement;
    let sidebar_close: HTMLButtonElement;

    onMount(() => {
        sidebar.style.width = "300px"
        sidebar_close.style.transform = "rotate(180deg)"
    })

    function handleSidebar() {
        sidebar_close.style.transform = "rotate(" + String(Number(sidebar_close.style.transform.slice(7, -4)) + 180) + "deg)"
        if (Number(sidebar.style.width.slice(0, -2)) > 30) {
            sidebar.style.width = "30px"
            sidebar.childNodes.forEach(element => {
                if (element instanceof HTMLAnchorElement) {
                    element.style.display = "none"
                }
            });
        } else {
            sidebar.style.width = "300px"
            sidebar.childNodes.forEach(element => {
                if (element instanceof HTMLAnchorElement) {
                    element.style.display = "block"
                }
            });
        }
    }
</script>

<div class="sidebar" bind:this={sidebar}>
    <button class="sidebar-close" bind:this={sidebar_close} on:click={handleSidebar}> > </button>
    <a href="/">Home</a>
    <a href="/TextCaptcha">Text Captchas</a>
</div>

<style lang="scss">
    .sidebar {
        background-color: rgb(228, 228, 228);
        width: 300px;
        height: 100%;
        position: absolute;
        left: 0;
        top: 0;
        transition: width 0.5s;

        a {
            display: block;
            width: 70%;
            text-decoration: none;
            color: black;
            padding: 10px;
            margin: 5px;
            padding-left: 25px;
            border: 3px solid darkgray;
        }
        a:hover {
            scale: 1.05;
        }
        a:focus {
            scale: 1;
            filter: brightness(0.8);
        }
    }
    .sidebar-close {
        background-color: rgb(202, 202, 202);
        padding: 10px;
        position: absolute;
        top: 0;
        right: 0;
        transform: rotate(180deg);
        transition: transform 0.5s;
    }
</style>