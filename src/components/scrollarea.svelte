<script>
    import { onMount, onDestroy } from "svelte";
    import { createEventDispatcher } from "svelte";

    export let content = [];
    export let itemHeight = 50;
    export let showScrollButtons = true;
    export let smoothScroll = true;
    export let infiniteScroll = false;
    export let loadMoreThreshold = 100; // Threshold in pixels before loading more content

    let scrollContainer;
    let isScrolling = false;
    let scrollTop = 0;
    let contentHeight = 0;
    let scrollContainerHeight = 0;
    let showTopButton = false;
    let showBottomButton = false;
    let dispatcher = createEventDispatcher();

    const handleScroll = () => {
        const { scrollTop, scrollHeight, clientHeight } = scrollContainer;
        scrollTop = scrollTop;
        contentHeight = scrollHeight;
        scrollContainerHeight = clientHeight;

        showTopButton = scrollTop > 100;
        showBottomButton = scrollTop + scrollContainerHeight < scrollHeight - 100;

        if (infiniteScroll && scrollHeight - (scrollTop + clientHeight) < loadMoreThreshold) {
            if (!isScrolling) {
                isScrolling = true;
                dispatcher("loadMore");
            }
        } else {
            isScrolling = false;
        }
    };

    const scrollToTop = () => {
        scrollContainer.scrollTo({
            top: 0,
            behavior: smoothScroll ? "smooth" : "auto"
        });
    };

    const scrollToBottom = () => {
        scrollContainer.scrollTo({
            top: scrollContainer.scrollHeight,
            behavior: smoothScroll ? "smooth" : "auto"
        });
    };

    onMount(() => {
        scrollContainer.addEventListener("scroll", handleScroll);
        contentHeight = scrollContainer.scrollHeight;
        scrollContainerHeight = scrollContainer.clientHeight;
    });

    onDestroy(() => {
        scrollContainer.removeEventListener("scroll", handleScroll);
    });
</script>

<style>
    .scroll-container {
        position: relative;
        overflow: auto;
        width: 100%;
        height: 100%;
        border: 1px solid #ddd;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }

    .scroll-content {
        padding: 10px;
        box-sizing: border-box;
    }

    .scroll-item {
        height: var(--item-height);
        line-height: var(--item-height);
        border-bottom: 1px solid #eee;
        padding: 0 10px;
        display: flex;
        align-items: center;
        transition: background-color 0.3s ease;
    }

    .scroll-item:nth-child(odd) {
        background-color: #f9f9f9;
    }

    .scroll-item:hover {
        background-color: #e0e0e0;
    }

    .scroll-button {
        position: fixed;
        right: 10px;
        width: 40px;
        height: 40px;
        background-color: #007bff;
        color: white;
        border: none;
        border-radius: 50%;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 18px;
        z-index: 10;
        opacity: 0.8;
        transition: opacity 0.3s ease;
    }

    .scroll-button:hover {
        opacity: 1;
    }

    .scroll-button-top {
        bottom: 60px;
    }

    .scroll-button-bottom {
        bottom: 10px;
    }
</style>

<div bind:this={scrollContainer} class="scroll-container">
    <div class="scroll-content" style="--item-height: {itemHeight}px">
        {#each content as item}
            <div class="scroll-item">
                {item}
            </div>
        {/each}
    </div>

    {#if showScrollButtons}
        {#if showTopButton}
            <button class="scroll-button scroll-button-top" on:click={scrollToTop}>
                ↑
            </button>
        {/if}
        {#if showBottomButton}
            <button class="scroll-button scroll-button-bottom" on:click={scrollToBottom}>
                ↓
            </button>
        {/if}
    {/if}
</div>

