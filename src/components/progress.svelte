<script>
    export let type = 'determinate'; // 'determinate', 'indeterminate'
    export let value = 0; // Current progress value (0-100)
    export let max = 100; // Maximum value for determinate progress
    export let color = '#4caf50'; // Progress bar color
    export let backgroundColor = '#e0e0e0'; // Background color of the progress bar
    export let height = '24px'; // Height of the progress bar
    export let animated = true; // Whether to use animation for indeterminate progress
    export let label = ''; // Label text to display
    export let ariaLabel = ''; // ARIA label for accessibility
    export let ariaValueText = ''; // ARIA value text for accessibility

    const isDeterminate = type === 'determinate';
    const progressPercentage = (value / max) * 100;
    const indeterminateAnimationDuration = 2; // Duration for indeterminate animation
</script>

<style>
    .progress-container {
        position: relative;
        width: 100%;
        height: var(--height);
        background-color: var(--background-color);
        border-radius: 4px;
        overflow: hidden;
    }

    .progress-bar {
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        background-color: var(--color);
        transition: width 0.5s ease;
    }

    .progress-label {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        color: #fff;
        font-size: 14px;
        font-weight: bold;
    }

    .progress-indeterminate .progress-bar {
        width: 100%;
        background: linear-gradient(
            90deg,
            var(--color) 25%,
            transparent 50%,
            var(--color) 75%
        );
        background-size: 200% 100%;
        animation: indeterminate-shimmer var(--animation-duration) infinite;
    }

    @keyframes indeterminate-shimmer {
        0% {
            background-position: -200% 0;
        }
        100% {
            background-position: 200% 0;
        }
    }
</style>

<div
    class="progress-container"
    style="
        --height: {height};
        --background-color: {backgroundColor};
        --color: {color};
        --animation-duration: {indeterminateAnimationDuration}s;
    "
    role="progressbar"
    aria-valuenow={isDeterminate ? value : undefined}
    aria-valuemin={isDeterminate ? 0 : undefined}
    aria-valuemax={isDeterminate ? max : undefined}
    aria-label={ariaLabel}
    aria-valuetext={ariaValueText}
>
    <div
        class="progress-bar {type}"
        style="width: {isDeterminate ? progressPercentage + '%' : '100%'}"
    >
        {#if label}
            <div class="progress-label">{label}</div>
        {/if}
    </div>
</div>

