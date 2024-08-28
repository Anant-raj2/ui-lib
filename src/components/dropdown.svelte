<script>
</script>

<style>
    .dropdown-container {
        position: relative;
        width: 100%;
        max-width: 300px;
        font-family: Arial, sans-serif;
    }

    .dropdown-toggle {
        display: flex;
        align-items: center;
        justify-content: space-between;
        background-color: #fff;
        padding: 10px 15px;
        border: 1px solid #ccc;
        border-radius: 4px;
        cursor: pointer;
        user-select: none;
        position: relative;
        transition: border-color 0.3s ease;
    }

    .dropdown-toggle.disabled {
        background-color: #f5f5f5;
        cursor: not-allowed;
    }

    .dropdown-toggle:focus {
        outline: none;
        border-color: #007BFF;
    }

    .dropdown-toggle-icon {
        margin-right: 10px;
    }

    .dropdown-toggle-text {
        flex-grow: 1;
        text-align: left;
    }

    .dropdown-caret {
        transition: transform 0.3s ease;
    }

    .dropdown-caret.open {
        transform: rotate(180deg);
    }

    .dropdown-menu {
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background-color: #fff;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        max-height: 200px;
        overflow-y: auto;
        z-index: 10;
        transition: opacity 0.3s ease, transform 0.3s ease;
        transform-origin: top center;
    }

    .dropdown-menu.closed {
        opacity: 0;
        transform: scaleY(0);
        pointer-events: none;
    }

    .dropdown-item {
        padding: 10px 15px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }

    .dropdown-item:hover,
    .dropdown-item.focused {
        background-color: #007BFF;
        color: #fff;
    }

    .dropdown-item.selected {
        background-color: #007BFF;
        color: #fff;
        font-weight: bold;
    }

    .dropdown-item.disabled {
        background-color: #f5f5f5;
        color: #ccc;
        cursor: not-allowed;
    }

</style>

<div class="dropdown-container" on:keydown={handleKeydown}>
    <div
        class="dropdown-toggle {disabled ? 'disabled' : ''}"
        tabindex="0"
        on:click={toggleDropdown}
        aria-haspopup="true"
        aria-expanded={isOpen}
    >
        {#if icon}
            <img src={icon} alt="Icon" class="dropdown-toggle-icon" />
        {/if}
        <span class="dropdown-toggle-text">
            {#if multiSelect}
                {#if selectedItems.length > 0}
                    {selectedItems.map((item) => item.label).join(", ")}
                {:else}
                    {placeholder}
                {/if}
            {:else}
                {selectedItem ? selectedItem.label : placeholder}
            {/if}
        </span>
        <span class="dropdown-caret {isOpen ? 'open' : ''}">▼</span>
    </div>

    <div class="dropdown-menu {isOpen ? '' : 'closed'}">
        {#if searchEnabled}
            <div class="dropdown-search">
                <input
                    type="text"
                    placeholder="Search..."
                    on:input={handleSearch}
                    value={searchQuery}
                />
            </div>
        {/if}
        {#each filteredItems as item, index}
            <div
                class="dropdown-item {selectedItem === item || selectedItems.includes(item) ? 'selected' : ''} {focusedIndex === index ? 'focused' : ''}"
                on:click={() => handleItemClick(item)}
                on:mouseenter={() => (focusedIndex = index)}
                aria-selected={selectedItem === item || selectedItems.includes(item)}
            >
                {item.label}
            </div>
        {/each}
    </div>

</div>
