<script>
    export let imageUrl = ''; // URL of the avatar image
    export let text = ''; // Text to display if no image
    export let size = 'medium'; // Size of the avatar ('small', 'medium', 'large')
    export let shape = 'circle'; // Shape of the avatar ('circle', 'square')
    export let border = false; // Whether to show a border
    export let borderColor = '#fff'; // Color of the border
    export let shadow = false; // Whether to show a shadow
    export let status = ''; // Status to display ('online', 'offline', 'away', etc.)
    export let tooltip = ''; // Tooltip text to display on hover
    export let ariaLabel = ''; // ARIA label for accessibility

    // Determine avatar classes based on props
    const avatarClass = `avatar ${size} ${shape} ${border ? 'border' : ''} ${shadow ? 'shadow' : ''}`;
    const statusClass = status ? `status ${status}` : '';
</script>

<style>
    .avatar {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        font-size: var(--avatar-font-size);
        color: var(--avatar-color);
        background-color: var(--avatar-background);
        border-radius: var(--avatar-radius);
        width: var(--avatar-size);
        height: var(--avatar-size);
        text-align: center;
        line-height: var(--avatar-size);
        position: relative;
        overflow: hidden;
        box-sizing: border-box;
        transition: box-shadow 0.3s ease, border-color 0.3s ease;
    }

    .avatar img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        border-radius: var(--avatar-radius);
    }

    .avatar.border {
        border: 2px solid var(--border-color);
    }

    .avatar.shadow {
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .avatar.circle {
        border-radius: 50%;
    }

    .avatar.square {
        border-radius: 4px;
    }

    .avatar.small {
        --avatar-size: 32px;
        --avatar-font-size: 14px;
        --avatar-color: #fff;
        --avatar-background: #888;
        --avatar-radius: 50%;
        --border-color: #ccc;
    }

    .avatar.medium {
        --avatar-size: 48px;
        --avatar-font-size: 18px;
        --avatar-color: #fff;
        --avatar-background: #555;
        --avatar-radius: 50%;
        --border-color: #bbb;
    }

    .avatar.large {
        --avatar-size: 64px;
        --avatar-font-size: 24px;
        --avatar-color: #fff;
        --avatar-background: #333;
        --avatar-radius: 50%;
        --border-color: #aaa;
    }

    .status {
        position: absolute;
        width: 12px;
        height: 12px;
        border-radius: 50%;
        bottom: 0;
        right: 0;
        border: 2px solid #fff;
    }

    .status.online {
        background-color: #4caf50;
    }

    .status.offline {
        background-color: #f44336;
    }

    .status.away {
        background-color: #ff9800;
    }

    .avatar.tooltip:hover::after {
        content: attr(data-tooltip);
        position: absolute;
        bottom: 100%;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0, 0, 0, 0.8);
        color: #fff;
        padding: 4px 8px;
        border-radius: 4px;
        font-size: 12px;
        white-space: nowrap;
        opacity: 1;
        pointer-events: none;
        transition: opacity 0.3s ease;
    }

    .avatar.tooltip {
        position: relative;
    }
</style>

<div
    class={avatarClass}
    style="--border-color: {borderColor};"
    aria-label={ariaLabel}
    data-tooltip={tooltip}
    class={tooltip ? 'tooltip' : ''}
>
    {#if imageUrl}
        <img src={imageUrl} alt="" />
    {:else}
        {text}
    {/if}
    {#if status}
        <div class={statusClass}></div>
    {/if}
</div>
