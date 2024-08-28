<script>
    import { createEventDispatcher } from 'svelte';

    export let checked = false; // Whether the switch is in the 'on' state
    export let indeterminate = false; // Whether the switch is in an indeterminate state
    export let disabled = false; // Whether the switch is disabled
    export let size = 'medium'; // Size of the switch ('small', 'medium', 'large')
    export let color = '#4caf50'; // Color of the switch when 'on'
    export let offColor = '#e0e0e0'; // Color of the switch when 'off'
    export let handleColor = '#fff'; // Color of the handle
    export let labelOn = 'On'; // Label when switch is 'on'
    export let labelOff = 'Off'; // Label when switch is 'off'
    export let ariaLabel = ''; // ARIA label for accessibility
    export let ariaDescribedBy = ''; // ARIA described by attribute
    export let ariaChecked = '';

    const dispatch = createEventDispatcher();

    const toggleSwitch = () => {
        if (!disabled) {
            checked = !checked;
            dispatch('change', { checked });
        }
    };

    $: ariaChecked = indeterminate ? 'mixed' : checked ? 'true' : 'false';
</script>

<style>
    .switch-container {
        display: flex;
        align-items: center;
        cursor: pointer;
        user-select: none;
    }

    .switch-label {
        margin-right: 8px;
        font-size: 14px;
        color: #333;
    }

    .switch {
        position: relative;
        display: inline-flex;
        align-items: center;
        height: var(--switch-height);
        width: var(--switch-width);
        background-color: var(--off-color);
        border-radius: var(--switch-radius);
        transition: background-color 0.3s;
        opacity: {disabled ? 0.6 : 1};
        cursor: {disabled ? 'not-allowed' : 'pointer'};
    }

    .switch-on {
        background-color: var(--color);
    }

    .switch-handle {
        position: absolute;
        height: var(--handle-size);
        width: var(--handle-size);
        border-radius: 50%;
        background-color: var(--handle-color);
        transition: transform 0.3s;
        transform: translateX(var(--handle-position));
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }

    .switch-indeterminate .switch-handle {
        width: calc(var(--handle-size) * 1.5);
        transform: translateX(calc(var(--handle-position) + 10px));
    }

    .switch-disabled {
        pointer-events: none;
    }

    .switch-small {
        --switch-width: 40px;
        --switch-height: 20px;
        --switch-radius: 10px;
        --handle-size: 18px;
        --handle-position: calc(var(--switch-width) / 2 - var(--handle-size) / 2);
    }

    .switch-medium {
        --switch-width: 60px;
        --switch-height: 30px;
        --switch-radius: 15px;
        --handle-size: 26px;
        --handle-position: calc(var(--switch-width) / 2 - var(--handle-size) / 2);
    }

    .switch-large {
        --switch-width: 80px;
        --switch-height: 40px;
        --switch-radius: 20px;
        --handle-size: 34px;
        --handle-position: calc(var(--switch-width) / 2 - var(--handle-size) / 2);
    }
</style>

<div
    class="switch-container"
    on:click={toggleSwitch}
    role="switch"
    aria-checked={ariaChecked}
    aria-label={ariaLabel}
    aria-describedby={ariaDescribedBy}
>
    <div class="switch {size} {checked ? 'switch-on' : ''} {indeterminate ? 'switch-indeterminate' : ''} {disabled ? 'switch-disabled' : ''}">
        <div class="switch-handle"></div>
    </div>
    {#if labelOn && checked}
        <span class="switch-label">{labelOn}</span>
    {:else if labelOff && !checked}
        <span class="switch-label">{labelOff}</span>
    {/if}
</div>

