<script lang="ts">
    export let position: 'relative' | 'absolute' | 'fixed' = 'relative';
    export let containerStyles: string = '';

    type MouseMoveEvent = {
        currentTarget: HTMLElement;
        clientX: number;
        clientY: number;
    }

    const handleMouseMove = (event: MouseMoveEvent): void => {
        const { currentTarget: target } = event;
        const rect = target!.getBoundingClientRect();
        const x = event.clientX - rect.left;
        const y = event.clientY - rect.top;
        target.style.setProperty('--mouse-x', `${x}px`);
        target.style.setProperty('--mouse-y', `${y}px`);
    }
</script>

<div
    class={`radial-hover ${position} ${containerStyles}`}
    on:mousemove={handleMouseMove}
>
    <slot />
</div>

<style>
    .radial-hover::after {
        opacity: 0;
        background: radial-gradient(
            120rem circle at var(--mouse-x) var(--mouse-y),
            rgba(255,255,255,0.04),
            transparent 50%
        );
        border-radius: inherit;
        content: "";
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
    }
    .radial-hover:hover::after {
        opacity: 1;
    }
</style>