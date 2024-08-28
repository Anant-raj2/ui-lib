<script>
    import { createEventDispatcher } from 'svelte';

    export let type = 'text'; // Type of input (text, password, email, etc.)
    export let value = ''; // Value of the input
    export let placeholder = ''; // Placeholder text
    export let label = ''; // Label text
    export let disabled = false; // Whether the input is disabled
    export let readonly = false; // Whether the input is read-only
    export let size = 'medium'; // Size of the input ('small', 'medium', 'large')
    export let icon = ''; // Icon to display inside the input
    export let iconPosition = 'left'; // Icon position ('left', 'right')
    export let ariaLabel = ''; // ARIA label for accessibility
    export let ariaDescribedBy = ''; // ARIA described by attribute

    const dispatch = createEventDispatcher();

    const handleInput = (event) => {
        value = event.target.value;
        dispatch('input', { value });
    };

    const handleFocus = (event) => {
        dispatch('focus', event);
    };

    const handleBlur = (event) => {
        dispatch('blur', event);
    };
</script>

<style>
    .input-container {
        position: relative;
        display: flex;
        align-items: center;
        width: 100%;
    }

    .input-label {
        margin-bottom: 4px;
        font-size: 14px;
        color: #333;
        display: block;
    }

    .input-field {
        width: 100%;
        padding: var(--input-padding);
        border: 1px solid var(--border-color);
        border-radius: 4px;
        font-size: 14px;
        color: #333;
        transition: border-color 0.3s, box-shadow 0.3s;
        background-color: var(--background-color);
        box-sizing: border-box;
        outline: none;
    }

    .input-field:focus {
        border-color: var(--focus-border-color);
        box-shadow: 0 0 4px rgba(0, 0, 0, 0.2);
    }

    .input-field.disabled {
        background-color: #f5f5f5;
        cursor: not-allowed;
    }

    .input-field.readonly {
        background-color: #f9f9f9;
        cursor: default;
    }

    .input-icon {
        position: absolute;
        font-size: 16px;
        color: #888;
    }

    .input-icon.left {
        left: 10px;
    }

    .input-icon.right {
        right: 10px;
    }

    .input-small {
        --input-padding: 8px;
        --border-color: #ccc;
        --background-color: #fff;
        --focus-border-color: #4caf50;
        font-size: 12px;
    }

    .input-medium {
        --input-padding: 12px;
        --border-color: #ccc;
        --background-color: #fff;
        --focus-border-color: #4caf50;
        font-size: 14px;
    }

    .input-large {
        --input-padding: 16px;
        --border-color: #ccc;
        --background-color: #fff;
        --focus-border-color: #4caf50;
        font-size: 16px;
    }
</style>

<div class="input-container">
    {#if label}
        <label class="input-label" for={type}>{label}</label>
    {/if}
    {#if icon && iconPosition === 'left'}
        <span class="input-icon left">{icon}</span>
    {/if}
    <input
        type={type}
        id={type}
        class="input-field {size} {disabled ? 'disabled' : ''} {readonly ? 'readonly' : ''}"
        placeholder={placeholder}
        bind:value={value}
        on:input={handleInput}
        on:focus={handleFocus}
        on:blur={handleBlur}
        disabled={disabled}
        readonly={readonly}
        aria-label={ariaLabel}
        aria-describedby={ariaDescribedBy}
    />
    {#if icon && iconPosition === 'right'}
        <span class="input-icon right">{icon}</span>
    {/if}
</div>
