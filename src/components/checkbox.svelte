<script>
    import { createEventDispatcher } from "svelte";

    export let checked = false;
    export let indeterminate = false;
    export let disabled = false;
    export let label = "";
    export let tooltip = "";
    export let name = "";
    export let value = "";

    const dispatch = createEventDispatcher();

    const handleChange = () => {
        if (disabled) return;

        if (indeterminate) {
            checked = true;
            indeterminate = false;
        } else {
            checked = !checked;
        }

        dispatch("change", { checked, indeterminate, name, value });
    };
</script>

<style>
    .checkbox-container {
        display: flex;
        align-items: center;
        cursor: pointer;
        position: relative;
        user-select: none;
    }

    .checkbox-container.disabled {
        cursor: not-allowed;
        opacity: 0.6;
    }

    .checkbox-input {
        display: none;
    }

    .checkbox-box {
        width: 18px;
        height: 18px;
        border: 2px solid #007BFF;
        border-radius: 4px;
        background-color: white;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: background-color 0.3s ease, border-color 0.3s ease;
    }

    .checkbox-container:hover .checkbox-box {
        border-color: #0056b3;
    }

    .checkbox-box.checked {
        background-color: #007BFF;
        border-color: #007BFF;
    }

    .checkbox-box.indeterminate {
        background-color: #ffc107;
        border-color: #ffc107;
    }

    .checkbox-icon {
        color: white;
        font-size: 12px;
        transition: opacity 0.3s ease;
    }

    .checkbox-icon.hidden {
        opacity: 0;
    }

    .checkbox-label {
        margin-left: 8px;
        font-size: 14px;
    }

    .tooltip {
        visibility: hidden;
        background-color: #333;
        color: #fff;
        text-align: center;
        border-radius: 4px;
        padding: 5px;
        position: absolute;
        z-index: 1;
        bottom: 125%; /* Position above the checkbox */
        left: 50%;
        transform: translateX(-50%);
        opacity: 0;
        transition: opacity 0.3s ease;
        font-size: 12px;
        white-space: nowrap;
    }

    .checkbox-container:hover .tooltip {
        visibility: visible;
        opacity: 1;
    }
</style>

<div
    class="checkbox-container {disabled ? 'disabled' : ''}"
    on:click={handleChange}
    aria-checked={indeterminate ? 'mixed' : checked}
    aria-disabled={disabled}
    role="checkbox"
    tabindex="0"
    on:keydown={(e) => e.key === 'Enter' || e.key === ' ' ? handleChange() : null}
>
    <input
        class="checkbox-input"
        type="checkbox"
        bind:checked
        disabled={disabled}
        aria-label={label}
    />
    <div
        class="checkbox-box {checked ? 'checked' : ''} {indeterminate ? 'indeterminate' : ''}"
    >
        {#if indeterminate}
            <span class="checkbox-icon">−</span>
        {:else if checked}
            <span class="checkbox-icon">✔</span>
        {:else}
            <span class="checkbox-icon hidden">✔</span>
        {/if}
    </div>
    {#if label}
        <label class="checkbox-label" for={name}>{label}</label>
    {/if}
    {#if tooltip}
        <div class="tooltip">{tooltip}</div>
    {/if}
</div>

