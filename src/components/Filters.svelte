<script>
    let { data = [], active = $bindable('') } = $props();

    const categories = $derived([...new Set(data.map((d) => d.category).filter(Boolean))]);

    function toggle(category) {
        const key = category.toLowerCase();
        active = active === key ? '' : key;
    }
</script>

<p>Filter by category:</p>
<div class="filters">
    {#each categories as category}
        <button
            type="button"
            class:off={active && active !== category.toLowerCase()}
            aria-pressed={active === category.toLowerCase()}
            onclick={() => toggle(category)}
        >{category}</button>
    {/each}
</div>

<style>
    .filters {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
        margin: 1rem 0;
    }
    button {
        background: var(--grey03);
        border: 2px solid var(--grey03);
        border-radius: 20px;
        color: #fff;
        cursor: pointer;
        font-family: 'BentonSansCond-Regular', sans-serif;
        font-size: 1rem;
        padding: 4px 14px;
    }
    button.off {
        background: transparent;
        color: var(--grey03);
    }
</style>
