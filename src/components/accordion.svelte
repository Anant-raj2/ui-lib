<script>
  import { slide } from 'svelte/transition';
  import { createEventDispatcher } from 'svelte';

  export let items = [];
  export let multiExpand = false;

  let activeItems = new Set();
  const dispatch = createEventDispatcher();

  function toggleItem(item) {
    if (multiExpand) {
      if (activeItems.has(item)) {
        activeItems.delete(item);
      } else {
        activeItems.add(item);
      }
      activeItems = activeItems; // Trigger reactivity
    } else {
      activeItems = new Set(activeItems.has(item) ? [] : [item]);
    }
    dispatch('change', { activeItems: Array.from(activeItems) });
  }
</script>

<div class="accordion">
  {#each items as item, index}
    <div class="accordion-item">
      <button
        class="accordion-header"
        on:click={() => toggleItem(item)}
        aria-expanded={activeItems.has(item)}
      >
        {item.title}
        <span class="icon">{activeItems.has(item) ? '−' : '+'}</span>
      </button>
      {#if activeItems.has(item)}
        <div class="accordion-content" transition:slide="{{ duration: 300 }}">
          {item.content}
        </div>
      {/if}
    </div>
  {/each}
</div>

<style>
  .accordion {
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  .accordion-item {
    border-bottom: 1px solid #ddd;
  }

  .accordion-item:last-child {
    border-bottom: none;
  }

  .accordion-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    padding: 15px;
    background: none;
    border: none;
    text-align: left;
    cursor: pointer;
  }

  .accordion-header:hover {
    background-color: #f0f0f0;
  }

  .icon {
    font-size: 1.2em;
  }

  .accordion-content {
    padding: 15px;
  }
</style>
