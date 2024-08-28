<script>
    import ComplexSkeleton from './ComplexSkeleton.svelte';

    let isLoading = true;
    let items = Array(5).fill(0); // Array of 5 items for demonstration

    // Simulate data loading
    setTimeout(() => {
        isLoading = false;
    }, 3000);
</script>

<main>
    {#if isLoading}
        <ComplexSkeleton
            type="rectangle"
            width="100%"
            height="20px"
            count={items.length}
            shapeRadius="4px"
            animationDuration={1.5}
            backgroundColor="#e0e0e0"
            shimmerColor="#f5f5f5"
        />
    {:else}
        <div>
            <!-- Render actual content here -->
            {#each items as item}
                <div>Item {item}</div>
            {/each}
        </div>
    {/if}
</main>
