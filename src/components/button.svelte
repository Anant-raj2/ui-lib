<script>
    import { createEventDispatcher } from 'svelte';

    export let type = 'primary'; // Button type ('primary', 'secondary', 'tertiary')
    export let size = 'medium'; // Button size ('small', 'medium', 'large')
    export let loading = false; // Whether the button is in loading state
    export let disabled = false; // Whether the button is disabled
    export let icon = ''; // Optional icon to display in the button
    export let iconPosition = 'left'; // Icon position ('left', 'right')
    export let ariaLabel = ''; // ARIA label for accessibility
    export let ariaDescribedBy = ''; // ARIA described by attribute
    export let onClick = () => {}; // Click event handler

    const dispatch = createEventDispatcher();

    const handleClick = (event) => {
        if (!disabled && !loading) {
            dispatch('click', event);
            onClick(event);
        }
    };
</script>

<style>
    .button {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        padding: var(--button-padding);
        border: none;
        border-radius: var(--button-radius);
        font-size: var(--button-font-size);
        font-weight: bold;
        cursor: {disabled ? 'not-allowed' : 'pointer'};
        transition: background-color 0.3s, color 0.3s, transform 0.3s;
        outline: none;
        box-shadow: var(--button-shadow);
        background-color: var(--button-background);
        color: var(--button-color);
    }

    .button:disabled {
        background-color: #d0d0d0;
        color: #a0a0a0;
    }

    .button-loading {
        pointer-events: none;
        opacity: 0.7;
    }

    .button-icon {
        margin: 0 8px;
        font-size: var(--icon-size);
    }

    .button-primary {
        --button-background: #007bff;
        --button-color: #fff;
        --button-padding: 12px 24px;
        --button-radius: 4px;
        --button-font-size: 16px;
        --icon-size: 20px;
        --button-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .button-secondary {
        --button-background: #6c757d;
        --button-color: #fff;
        --button-padding: 12px 24px;
        --button-radius: 4px;
        --button-font-size: 16px;
        --icon-size: 20px;
        --button-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .button-tertiary {
        --button-background: #fff;
        --button-color: #007bff;
        --button-padding: 12px 24px;
        --button-radius: 4px;
        --button-font-size: 16px;
        --icon-size: 20px;
        --button-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        border: 1px solid #007bff;
    }

    .button-small {
        --button-padding: 8px 16px;
        --button-font-size: 14px;
        --icon-size: 16px;
    }

    .button-medium {
        --button-padding: 12px 24px;
        --button-font-size: 16px;
        --icon-size: 20px;
    }

    .button-large {
        --button-padding: 16px 32px;
        --button-font-size: 18px;
        --icon-size: 24px;
    }

    .button:hover:not(:disabled) {
        transform: scale(1.05);
    }

    .button:active:not(:disabled) {
        transform: scale(0.95);
    }

    .button-spinner {
        border: 4px solid rgba(0, 0, 0, 0.1);
        border-radius: 50%;
        border-top: 4px solid #fff;
        width: 24px;
        height: 24px;
        animation: spin 1s linear infinite;
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }
</style>

<button
    class="button button-{type} {size} {loading ? 'button-loading' : ''}"
    on:click={handleClick}
    aria-label={ariaLabel}
    aria-describedby={ariaDescribedBy}
    disabled={disabled}
>
    {#if loading}
        <div class="button-spinner"></div>
    {/if}
    {#if icon && iconPosition === 'left'}
        <span class="button-icon">{icon}</span>
    {/if}
    <slot></slot>
    {#if icon && iconPosition === 'right'}
        <span class="button-icon">{icon}</span>
    {/if}
</button>

