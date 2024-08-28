<script>
    export let type = 'rectangle'; // 'rectangle', 'circle', 'custom'
    export let width = '100%'; // Width of the skeleton
    export let height = '20px'; // Height of the skeleton
    export let count = 1; // Number of skeleton items
    export let shapeRadius = '4px'; // Radius for circle or rounded corners
    export let animationDuration = 1.5; // Animation duration in seconds
    export let backgroundColor = '#e0e0e0'; // Background color of the skeleton
    export let shimmerColor = '#f5f5f5'; // Shimmer color
    export let customShape = ''; // Custom shape for advanced styling
</script>

<style>
    .skeleton-container {
        display: flex;
        flex-direction: column;
        gap: 10px;
        width: 100%;
    }

    .skeleton {
        background: linear-gradient(
            90deg,
            var(--background-color) 25%,
            var(--shimmer-color) 50%,
            var(--background-color) 75%
        );
        background-size: 200% 100%;
        animation: shimmer var(--animation-duration) infinite;
        border-radius: var(--shape-radius);
        width: var(--width);
        height: var(--height);
    }

    .skeleton.circle {
        border-radius: 50%;
    }

    .skeleton.custom {
        var(--custom-shape);
    }

    @keyframes shimmer {
        0% {
            background-position: -200% 0;
        }
        100% {
            background-position: 200% 0;
        }
    }
</style>

<div class="skeleton-container">
    {#each Array(count) as _, index}
        <div class="skeleton {type}" style="
            --width: {width};
            --height: {height};
            --background-color: {backgroundColor};
            --shimmer-color: {shimmerColor};
            --animation-duration: {animationDuration}s;
            --shape-radius: {shapeRadius};
            --custom-shape: {customShape};
        "></div>
    {/each}
</div>

