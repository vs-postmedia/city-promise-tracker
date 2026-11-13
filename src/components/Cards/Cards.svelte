<script>
    import './Cards.css';
    import { onMount } from 'svelte';

    // COMPONENTS
    import Tooltip from '$components/Tooltip.svelte';

    let { data = [], value = '', active = '', activeStatus = '' } = $props();

    // VARS
    let tooltipData = $state();
    let width = $state(500);
    let height = $state(500);
    let openCards = $state({});

    const margin = {
		top: 20,
		right: 20,
		bottom: 20,
		left: 20 
	};

    $effect(() => {
        // console.log(data);
    });

    // FUNCTIONS
    function toggle(i) {
        const willOpen = !openCards[i];
        openCards = willOpen ? { [i]: true } : {};
    }
    function openLink(event, url) {
        event.stopPropagation();
        console.log(url)
        if (url) window.open(url, '_blank', 'noopener');
    }
    function onKeydown(event, i) {
        if (event.key === 'Enter' || event.key === ' ') {
            event.preventDefault();
            toggle(i);
        }
    }
</script>

<div class="cards-container" bind:clientWidth={width}>
    {#each data as entry, i}
        <div
            class="card {entry.status.replace(/ /g, '-').toLowerCase()} {entry.category.toLowerCase()}"
            class:open={openCards[i]}
            class:hidden={(active && active !== entry.category.toLowerCase()) ||
                (activeStatus && activeStatus !== entry.status.replace(/ /g, '-').toLowerCase())}
            role="button"
            tabindex="0"
            aria-expanded={!!openCards[i]}
            onclick={() => toggle(i)}
            onkeydown={(e) => e.target === e.currentTarget && onKeydown(e, i)}
        >
            <header>
                <div class="text">
                    <p class="category">{entry.category}</p>
                    <h2>{entry.title}</h2>
                </div>   
                <span style="font-size: 1.5rem; color:#FFF"> 
                    <i class="fa-solid {entry.icon}"></i>
                </span>
            </header>
            <p>{entry.description}</p>
            <p class="more-info"> Click for more info...</p>
            <div class="details">
                <div class="details-inner">
                    <h3>The latest</h3>
                    <p>{entry.progress}</p>
                    <div class="link-container">
                        {#if entry.url_latest}
                            <button type="button" onclick={(e) => openLink(e, entry.url_latest)}>Read the latest</button>
                        {/if}
                        {#if entry.url_all}
                            <button type="button" onclick={(e) => openLink(e, entry.url_all)}>Find all our coverage</button>
                        {/if}
                    </div>
                </div>
            </div>
        </div>
    {/each}
</div>

