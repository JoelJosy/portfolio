<script lang="ts">
	import gsap from "gsap";
    import { browser } from '$app/environment';

    interface Props {
        visible?: boolean;
    }

    let { visible = false }: Props = $props();
    let headerText: HTMLElement;

    $effect(() => {
        
    });
</script>

<div class="website-content">
    <div class="header" bind:this={headerText}>
        <div class="h1">
            <h1>Website</h1>
            <h1>Content</h1>
        </div>
        <div class="header-revealer"></div>
    </div>
</div>

<style>
    .website-content {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow: hidden;
    }

    .header {
        position: relative;
        width: max-content;
        height: max-content;
    }
    .h1 {
        display: flex;
    }
    h1 {
        font-size: 80px;
        line-height: 1.1;
    }

    .header .h1 h1 {
        text-align: center;
        position: relative;
        margin: 0 10px;
        text-transform: uppercase;
        font-weight: 400;
    }

    /* Style for split text characters */
    :global(.char) {
        display: inline-block;
        transform-origin: bottom center;
    }

    .header-revealer {
        position: absolute;
        top: 0;
        width: 100%;
        height: 100%;
    }
    .header-revealer::after {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 105%;
        height: 120%;
        background: #fff;
        top: 80px;
    }
</style>
