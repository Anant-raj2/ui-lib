<script>
    import { onDestroy } from 'svelte';

    export let type = 'info'; // Type of the alert ('success', 'error', 'warning', 'info')
    export let message = ''; // Message to display in the alert
    export let dismissible = true; // Whether the alert can be dismissed
    export let autoDismiss = false; // Whether the alert should auto-dismiss after timeout
    export let autoDismissTime = 5000; // Timeout duration for auto-dismiss in milliseconds
    export let icon = ''; // Optional icon to display in the alert
    export let ariaLabel = ''; // ARIA label for accessibility
    export let ariaLive = 'assertive'; // ARIA live region setting ('assertive', 'polite', 'off')

    let visible = true;

    const alertTypes = {
        success: { color: '#4caf50', icon: '✓' },
        error: { color: '#f44336', icon: '✗' },
        warning: { color: '#ff9800', icon: '⚠' },
        info: { color: '#2196f3', icon: 'ℹ' }
    };

    const closeAlert = () => {
        visible = false;
    };

    if (autoDismiss) {
        const timeoutId = setTimeout(() => {
            closeAlert();
        }, autoDismissTime);

        onDestroy(() => {
            clearTimeout(timeoutId);
        });
    }
</script>

<style>
    .alert-container {
        display: {visible ? 'block' : 'none'};
        padding: 16px;
        border-radius: 4px;
        margin: 10px 0;
        position: relative;
        color: #fff;
        background-color: var(--alert-background);
        border-left: 5px solid var(--alert-border);
        font-size: 16px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        transition: opacity 0.3s ease-out;
    }

    .alert-dismissible .alert-close {
        position: absolute;
        top: 10px;
        right: 10px;
        background: none;
        border: none;
        color: #fff;
        font-size: 20px;
        cursor: pointer;
    }

    .alert-dismissible .alert-close:hover {
        color: #ccc;
    }

    .alert-success {
        --alert-background: #4caf50;
        --alert-border: #388e3c;
    }

    .alert-error {
        --alert-background: #f44336;
        --alert-border: #c62828;
    }

    .alert-warning {
        --alert-background: #ff9800;
        --alert-border: #f57f17;
    }

    .alert-info {
        --alert-background: #2196f3;
        --alert-border: #1976d2;
    }

    .alert-icon {
        font-size: 24px;
        margin-right: 10px;
    }

    .alert-content {
        display: flex;
        align-items: center;
    }

    .alert-message {
        flex: 1;
    }
</style>

<div
    class="alert-container alert-{type} {dismissible ? 'alert-dismissible' : ''}"
    role="alert"
    aria-label={ariaLabel}
    aria-live={ariaLive}
>
    {#if icon}
        <span class="alert-icon">{icon}</span>
    {/if}
    <div class="alert-content">
        <div class="alert-message">{message}</div>
        {#if dismissible}
            <button class="alert-close" on:click={closeAlert} aria-label="Close alert">
                &times;
            </button>
        {/if}
    </div>
</div>

