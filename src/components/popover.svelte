<script>
    import { onMount, onDestroy, createEventDispatcher } from 'svelte';
    import { tick } from 'svelte';

    export let trigger = 'click'; // 'click', 'hover', 'focus'
    export let placement = 'top'; // 'top', 'right', 'bottom', 'left'
    export let content = '';
    export let open = false;
    export let closeOnOutsideClick = true;
    export let closeOnEscape = true;
    export let animationDuration = 300; // in milliseconds

    let popover, triggerElement;
    let isVisible = open;
    const dispatch = createEventDispatcher();

    const showPopover = () => {
        isVisible = true;
        dispatch('open');
    };

    const hidePopover = () => {
        isVisible = false;
        dispatch('close');
    };

    const handleDocumentClick = (event) => {
        if (popover && !popover.contains(event.target) && !triggerElement.contains(event.target)) {
            hidePopover();
        }
    };

    const handleEscapeKey = (event) => {
        if (event.key === 'Escape') {
            hidePopover();
        }
    };

    const handleTriggerClick = () => {
        if (trigger === 'click') {
            if (isVisible) {
                hidePopover();
            } else {
                showPopover();
            }
        }
    };

</script>

<style>
    .popover {
        position: absolute;
        background: white;
        border: 1px solid #ddd;
        border-radius: 4px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        padding: 10px;
        opacity: 0;
        visibility: hidden;
        transition: opacity var(--animation-duration) ease, visibility var(--animation-duration) ease;
        z-index: 1000;
    }

    .popover.show {
        opacity: 1;
        visibility: visible;
    }

    .popover.top {
        transform: translateY(-10px);
    }

    .popover.right {
        transform: translateX(10px);
    }

    .popover.bottom {
        transform: translateY(10px);
    }

    .popover.left {
        transform: translateX(-10px);
    }

    .trigger {
        display: inline-block;
        cursor: pointer;
    }
</style>

<div bind:this={triggerElement} class="trigger">
    <slot name="trigger">Trigger</slot>
</div>

<div bind:this={popover} class="popover {placement} {isVisible ? 'show' : ''}" style="--animation-duration: {animationDuration}ms">
    <slot name="content">{content}</slot>
</div>

