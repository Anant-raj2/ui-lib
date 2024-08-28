<script>
</script>

<style>
    .drawer-container {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        display: flex;
        align-items: flex-start;
        justify-content: flex-start;
        pointer-events: none;
    }

    .drawer {
        background-color: #fff;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        overflow-y: auto;
        transition: transform 0.3s ease-in-out;
        z-index: 1000;
        pointer-events: auto;
    }

    .drawer.left, .drawer.right {
        width: var(--drawer-size);
        height: 100%;
        top: 0;
        bottom: 0;
    }

    .drawer.left {
        left: 0;
        transform: translateX(-100%);
    }

    .drawer.right {
        right: 0;
        transform: translateX(100%);
    }

    .drawer.top, .drawer.bottom {
        width: 100%;
        height: var(--drawer-size);
        left: 0;
        right: 0;
    }

    .drawer.top {
        top: 0;
        transform: translateY(-100%);
    }

    .drawer.bottom {
        bottom: 0;
        transform: translateY(100%);
    }

    .drawer-content {
        padding: 20px;
    }

    .overlay {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.5);
        z-index: 999;
        opacity: 0;
        transition: opacity 0.3s ease-in-out;
    }

    .overlay.visible {
        opacity: 1;
    }

    /* Scrollbar styling */
    .drawer::-webkit-scrollbar {
        width: 8px;
    }

    .drawer::-webkit-scrollbar-track {
        background: #f1f1f1;
    }

    .drawer::-webkit-scrollbar-thumb {
        background: #888;
        border-radius: 4px;
    }

    .drawer::-webkit-scrollbar-thumb:hover {
        background: #555;
    }

    /* Accessibility: Focus styles */
    .drawer:focus {
        outline: none;
    }
</style>

<div class="drawer-container" style="--drawer-size: {size};">
    {#if overlay}
        <div class="overlay {isOpen ? 'visible' : ''}" on:click={closeDrawer}></div>
    {/if}

    <div
        bind:this={drawerElement}
        class="drawer {position}"
        tabindex="-1"
        role="dialog"
        aria-modal="true"
        aria-label="Drawer"
    >
        <div class="drawer-content">
            <slot></slot>
        </div>
    </div>
</div>

