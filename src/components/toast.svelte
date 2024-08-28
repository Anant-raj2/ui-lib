<script>
    import { onMount, onDestroy, createEventDispatcher } from 'svelte';
    import { tick } from 'svelte';

    export let type = 'info'; // 'success', 'error', 'info'
    export let message = '';
    export let autoDismiss = true;
    export let autoDismissTimeout = 5000; // in milliseconds
    export let showActions = false;
    export let actions = [];
    export let visible = true;
    export let animationDuration = 300; // in milliseconds

    const dispatch = createEventDispatcher();
    let toastElement;
    let timer;

    const handleActionClick = (action) => {
        if (action.callback) action.callback();
        dispatch('action', { action });
    };

    const dismissToast = () => {
        visible = false;
        dispatch('dismiss');
        clearTimeout(timer);
    };

    onMount(() => {
        if (autoDismiss) {
            timer = setTimeout(dismissToast, autoDismissTimeout);
        }

        tick().then(() => {
            if (toastElement) {
                toastElement.addEventListener('animationend', handleAnimationEnd);
            }
        });
    });

    onDestroy(() => {
        clearTimeout(timer);
        if (toastElement) {
            toastElement.removeEventListener('animationend', handleAnimationEnd);
        }
    });

    const handleAnimationEnd = () => {
        if (!visible && toastElement) {
            toastElement.style.display = 'none';
        }
    };
</script>

<style>
    .toast {
        position: fixed;
        top: 10px;
        right: 10px;
        background-color: white;
        border: 1px solid #ddd;
        border-radius: 4px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        padding: 10px 20px;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        opacity: 0;
        visibility: hidden;
        transition: opacity var(--animation-duration) ease, visibility var(--animation-duration) ease;
        z-index: 1000;
    }

    .toast.show {
        opacity: 1;
        visibility: visible;
    }

    .toast.success {
        border-color: #28a745;
        color: #28a745;
    }

    .toast.error {
        border-color: #dc3545;
        color: #dc3545;
    }

    .toast.info {
        border-color: #17a2b8;
        color: #17a2b8;
    }

    .toast-actions {
        margin-top: 10px;
    }

    .toast-action {
        margin-right: 10px;
        cursor: pointer;
        text-decoration: underline;
        color: inherit;
    }
</style>

<div bind:this={toastElement} class="toast {type} {visible ? 'show' : ''}" style="--animation-duration: {animationDuration}ms">
    <div class="toast-message">
        {message}
    </div>
    {#if showActions}
        <div class="toast-actions">
            {#each actions as action}
                <span class="toast-action" on:click={() => handleActionClick(action)}>
                    {action.label}
                </span>
            {/each}
        </div>
    {/if}
</div>

