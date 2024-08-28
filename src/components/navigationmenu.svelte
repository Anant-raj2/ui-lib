<script>
</script>

<style>
    :global(body) {
        margin: 0;
        font-family: Arial, sans-serif;
    }

    .nav-container {
        width: 100%;
        display: flex;
        flex-direction: column;
        background-color: #333;
        color: white;
        transition: max-width 0.3s ease;
    }

    .nav-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 10px 20px;
        background-color: #444;
        border-bottom: 1px solid #555;
    }

    .nav-logo {
        display: flex;
        align-items: center;
        cursor: pointer;
    }

    .nav-logo img {
        height: 30px;
        margin-right: 10px;
    }

    .nav-toggle {
        font-size: 24px;
        cursor: pointer;
        display: none;
    }

    .nav-menu {
        display: flex;
        flex-direction: column;
        overflow: hidden;
        transition: max-height 0.3s ease;
    }

    .nav-menu.collapsed {
        max-height: 0;
    }

    .nav-menu.open {
        max-height: 100vh;
    }

    .nav-item {
        padding: 10px 20px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }

    .nav-item:hover, .nav-item.active {
        background-color: #555;
    }

    .nav-item-icon {
        margin-right: 10px;
    }

    .nav-submenu {
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.3s ease;
    }

    .nav-submenu.open {
        max-height: 500px; /* Adjust as needed */
    }

    .nav-submenu-item {
        padding: 10px 40px;
        background-color: #444;
        cursor: pointer;
    }

    .nav-submenu-item:hover {
        background-color: #555;
    }

    .nav-footer {
        padding: 10px 20px;
        border-top: 1px solid #555;
        background-color: #444;
    }

    /* Responsive Styles */
    @media (max-width: 768px) {
        .nav-toggle {
            display: block;
        }

        .nav-menu {
            display: none;
        }

        .nav-menu.open {
            display: flex;
        }

        .nav-container.collapsed {
            max-width: 80px;
        }

        .nav-item, .nav-submenu-item {
            padding: 10px;
            justify-content: center;
        }

        .nav-item-text, .nav-submenu-item-text {
            display: none;
        }

        .nav-submenu-item {
            padding-left: 10px;
        }

        .nav-submenu {
            max-height: none;
        }
    }
</style>

<div class="nav-container {collapsible ? (isOpen ? 'open' : 'collapsed') : ''}">
    <div class="nav-header">
        <div class="nav-logo" on:click={toggleMenu}>
            {#if logo}
                <img src={logo} alt="Logo" />
            {/if}
            <span>My App</span>
        </div>
        {#if responsive}
            <div class="nav-toggle" on:click={toggleMenu}>
                ☰
            </div>
        {/if}
    </div>

    <div class="nav-menu {isOpen ? 'open' : 'collapsed'}">
        {#each menuItems as item}
            <div class="nav-item {item.route === currentRoute ? 'active' : ''}" on:click={() => handleItemClick(item)}>
                {#if item.icon}
                    <span class="nav-item-icon">{@html item.icon}</span>
                {/if}
                <span class="nav-item-text">{item.label}</span>
                {#if item.subMenu}
                    <span class="nav-item-caret" on:click={(e) => { e.stopPropagation(); handleSubMenuToggle(item); }}>
                        {item.isOpen ? '▲' : '▼'}
                    </span>
                {/if}
            </div>
            {#if item.subMenu}
                <div class="nav-submenu {item.isOpen ? 'open' : ''}">
                    {#each item.subMenu as subItem}
                        <div class="nav-submenu-item" on:click={() => handleItemClick(subItem)}>
                            <span class="nav-submenu-item-text">{subItem.label}</span>
                        </div>
                    {/each}
                </div>
            {/if}
        {/each}
    </div>
</div>

