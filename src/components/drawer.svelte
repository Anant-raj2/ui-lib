<script>
    import { onMount, onDestroy } from "svelte";

    export let isOpen = false;
    export let position = "left"; // Can be 'left', 'right', 'top', 'bottom'
    export let size = "300px"; // Width or height depending on position
    export let overlay = true; // Enable or disable background overlay
    export let closeOnEsc = true; // Enable closing the drawer with ESC key
    export let closeOnOutsideClick = true; // Enable closing the drawer when clicking outside

    let drawerElement;

    const openDrawer = () => {
        if (drawerElement) {
            drawerElement.style.transform = "translate(0, 0)";
        }
    };

    const closeDrawer = () => {
        if (drawerElement) {
            if (position === "left") {
                drawerElement.style.transform = `translateX(-100%)`;
            } else if (position === "right") {
                drawerElement.style.transform = `translateX(100%)`;
            } else if (position === "top") {
                drawerElement.style.transform = `translateY(-100%)`;
            } else if (position === "bottom") {
                drawerElement.style.transform = `translateY(100%)`;
            }
        }
        isOpen = false;
    };

    const handleKeyDown = (event) => {
        if (event.key === "Escape" && closeOnEsc) {
            closeDrawer();
        }
    };

    const handleOutsideClick = (event) => {
        if (drawerElement && !drawerElement.contains(event.target) && closeOnOutsideClick) {
            closeDrawer();
        }
    };

    onMount(() => {
        if (isOpen) {
            openDrawer();
        }
        document.addEventListener("keydown", handleKeyDown);
        document.addEventListener("click", handleOutsideClick);

        return () => {
            document.removeEventListener("keydown", handleKeyDown);
            document.removeEventListener("click", handleOutsideClick);
        };
    });

    $: if (isOpen) {
        openDrawer();
    } else {
        closeDrawer();
    }
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

