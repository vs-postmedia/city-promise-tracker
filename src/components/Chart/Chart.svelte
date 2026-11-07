<script>
    import './Chart.css';
    import { onMount } from 'svelte';

    // COMPONENTS
    import Tooltip from '$components/Tooltip.svelte';

    let { data = [], value = '', activeStatus = $bindable('') } = $props();

    // VARS
    let tooltipData = $state();
    let width = $state(500);
    let height = $state(500);

    const margin = {
		top: 20,
		right: 20,
		bottom: 20,
		left: 20 
	};

    const groups = $derived.by(() => {
        const counts = new Map();
        for (const d of data) {
            if (!d.status) continue;
            counts.set(d.status, (counts.get(d.status) ?? 0) + 1);
        }
        const total = data.length || 1;

        return [...counts].sort((a, b) => b[1] - a[1]).map(([status, count]) => ({
            status,
            count,
            slug: status.replace(/ /g, '-').toLowerCase(),
            percent: Math.round((count / total) * 100)
        }));
    });

    // FUNCTIONS
    function toggleStatus(slug) {
        activeStatus = activeStatus === slug ? '' : slug;
    }
    function init() {
        // console.log('CHART INIT!')
    }
    function addCommasToNumber(number) {
        return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }

    // LIGHTS! CAMERA! ACTION!
    onMount(init);
</script>

<div class="chart-container" bind:clientWidth={width}>
    <div class="percentage-view">
        <ul class="legend">
            {#each groups as group}
                <li>
                    <button
                        type="button"
                        class="legend-btn"
                        class:dim={activeStatus && activeStatus !== group.slug}
                        aria-pressed={activeStatus === group.slug}
                        onclick={() => toggleStatus(group.slug)}
                    >
                        <span class="swatch {group.slug}"></span>
                        {group.status} ({group.count})
                    </button>
                </li>
            {/each}
        </ul>
        <div class="percentage-bar">
            {#each groups as group}
                <button
                    type="button"
                    class="segment {group.slug}"
                    class:dim={activeStatus && activeStatus !== group.slug}
                    style="width: {group.percent}%"
                    title="{group.status}: {group.count} ({group.percent}%)"
                    aria-pressed={activeStatus === group.slug}
                    onclick={() => toggleStatus(group.slug)}
                >
                    <span class="pct">{group.percent}%</span>
                </button>
            {/each}
        </div>
    </div>
</div>


