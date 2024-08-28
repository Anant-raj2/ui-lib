<script>
    import { onMount } from "svelte";
    import { createEventDispatcher } from "svelte";

    export let isOpen = false;
    export let title = "Collapsible Section";
    export let transitionDuration = 300; // Duration of the open/close animation in milliseconds
    export let disableToggleIcon = false; // Option to disable the toggle icon

    let contentElement;
    let contentHeight = 0;
    let contentStyles = "";

    const dispatch = createEventDispatcher();

    const toggleCollapse = () => {
        isOpen = !isOpen;
        updateContentStyles();
        dispatch("toggle", { isOpen });
    };

    const updateContentStyles = () => {
        if (isOpen) {
            contentStyles = `max-height: ${contentHeight}px;`;
        } else {
            contentStyles = "max-height: 0;";
        }
    };

    onMount(() => {
        contentHeight = contentElement.scrollHeight;
        updateContentStyles();
    });
</script>

<style>
    .collapsible {
        margin-bottom: 10px;
        border: 1px solid #ddd;
        border-radius: 4px;
        overflow: hidden;
        transition: box-shadow 0.3s ease-in-out;
    }

    .collapsible.open {
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    }

    .collapsible-header {
        background-color: #007BFF;
        color: white;
        padding: 12px 20px;
        cursor: pointer;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-weight: bold;
        font-size: 16px;
    }

    .collapsible-content {
        overflow: hidden;
        transition: max-height 0.3s ease-in-out;
    }

    .collapsible-content-inner {
        padding: 15px 20px;
        background-color: #f9f9f9;
    }

    .toggle-icon {
        font-size: 18px;
        transition: transform 0.3s ease-in-out;
    }

    .toggle-icon.open {
        transform: rotate(180deg);
    }

    /* Nested collapsibles */
    .nested-collapsible {
        margin-top: 10px;
    }

    /* Focus styles for accessibility */
    .collapsible-header:focus {
        outline: 3px solid #0056b3;
    }
</style>

<div class="collapsible {isOpen ? 'open' : ''}">
    <div
        class="collapsible-header"
        tabindex="0"
        role="button"
        aria-expanded={isOpen}
        on:click={toggleCollapse}
        on:keydown={(e) => e.key === 'Enter' || e.key === ' ' ? toggleCollapse() : null}
    >
        <span>{title}</span>
        {#if !disableToggleIcon}
            <span class="toggle-icon {isOpen ? 'open' : ''}">&darr;</span>
        {/if}
    </div>
    <div
        bind:this={contentElement}
        class="collapsible-content"
        style="{contentStyles}; transition-duration: {transitionDuration}ms;"
    >
        <div class="collapsible-content-inner">
            <slot></slot>
        </div>
    </div>
</div>
