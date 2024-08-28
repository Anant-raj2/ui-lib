<script>
    import { createEventDispatcher } from 'svelte';

    export let currentPage = 1; // Current active page
    export let totalPages = 10; // Total number of pages
    export let visiblePages = 5; // Number of visible pages in the pagination
    export let showFirstLast = true; // Whether to show "First" and "Last" buttons
    export let showPrevNext = true; // Whether to show "Previous" and "Next" buttons

    const dispatch = createEventDispatcher();

    // Calculate visible page range
    $: pageNumbers = calculatePageNumbers();

    function calculatePageNumbers() {
        const startPage = Math.max(1, currentPage - Math.floor(visiblePages / 2));
        const endPage = Math.min(totalPages, startPage + visiblePages - 1);

        let pages = [];

        if (startPage > 1) {
            pages.push(1);
            if (startPage > 2) pages.push('...');
        }

        for (let i = startPage; i <= endPage; i++) {
            pages.push(i);
        }

        if (endPage < totalPages) {
            if (endPage < totalPages - 1) pages.push('...');
            pages.push(totalPages);
        }

        return pages;
    }

    function goToPage(page) {
        if (page === '...') return;
        currentPage = Math.max(1, Math.min(totalPages, page));
        dispatch('pageChange', currentPage);
    }

    function prevPage() {
        if (currentPage > 1) goToPage(currentPage - 1);
    }

    function nextPage() {
        if (currentPage < totalPages) goToPage(currentPage + 1);
    }
</script>

<style>
    .pagination {
        display: flex;
        align-items: center;
        list-style: none;
        padding: 0;
        margin: 0;
        font-size: 14px;
    }

    .pagination li {
        margin: 0 4px;
    }

    .pagination a,
    .pagination button {
        display: block;
        padding: 8px 12px;
        text-decoration: none;
        color: #007bff;
        border: 1px solid #007bff;
        border-radius: 4px;
        cursor: pointer;
        transition: background-color 0.2s, color 0.2s;
    }

    .pagination a:hover,
    .pagination button:hover {
        background-color: #007bff;
        color: #fff;
    }

    .pagination .disabled {
        color: #ccc;
        border-color: #ccc;
        cursor: not-allowed;
    }

    .pagination .active {
        background-color: #007bff;
        color: #fff;
        border-color: #007bff;
    }

    .pagination .ellipsis {
        padding: 8px 12px;
    }
</style>

<ul class="pagination">
    {#if showFirstLast}
        <li>
            <button on:click={() => goToPage(1)} disabled={currentPage === 1} aria-label="First page">&laquo;&laquo;</button>
        </li>
    {/if}

    {#if showPrevNext}
        <li>
            <button on:click={prevPage} disabled={currentPage === 1} aria-label="Previous page">&laquo;</button>
        </li>
    {/if}

    {#each pageNumbers as page}
        <li>
            {#if page === '...'}
                <span class="ellipsis">...</span>
            {:else}
                <a
                    href="#"
                    class={page === currentPage ? 'active' : ''}
                    on:click={(e) => {
                        e.preventDefault();
                        goToPage(page);
                    }}
                    aria-label={`Page ${page}`}
                >
                    {page}
                </a>
            {/if}
        </li>
    {/each}

    {#if showPrevNext}
        <li>
            <button on:click={nextPage} disabled={currentPage === totalPages} aria-label="Next page">&raquo;</button>
        </li>
    {/if}

    {#if showFirstLast}
        <li>
            <button on:click={() => goToPage(totalPages)} disabled={currentPage === totalPages} aria-label="Last page">&raquo;&raquo;</button>
        </li>
    {/if}
</ul>
