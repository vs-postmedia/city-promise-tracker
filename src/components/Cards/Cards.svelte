<script>
    import './Cards.css';
    import { onMount } from 'svelte';

    // COMPONENTS
    import Tooltip from '$components/Tooltip.svelte';

    let { data = [], value = '', active = '' } = $props();

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
        console.log(data);
    });

    // FUNCTIONS
    function init() {
        console.log('CARDS INIT!')
    }

    function toggle(i) {
        openCards[i] = !openCards[i];
    }
    function onKeydown(event, i) {
        if (event.key === 'Enter' || event.key === ' ') {
            event.preventDefault();
            toggle(i);
        }
    }

    // LIGHTS! CAMERA! ACTION!
    onMount(init);
</script>

<div class="cards-container" bind:clientWidth={width}>
    {#each data as entry, i}
        <div
            class="card {entry.status.replace(/ /g, '-').toLowerCase()} {entry.category.toLowerCase()}"
            class:open={openCards[i]}
            class:hidden={active && active !== entry.category.toLowerCase()}
            role="button"
            tabindex="0"
            aria-expanded={!!openCards[i]}
            onclick={() => toggle(i)}
            onkeydown={(e) => onKeydown(e, i)}
        >
            <header>
                <p class="category">{entry.category}</p>
                <h2>{entry.title}</h2>
            </header>
            <p>{entry.description}</p>
            <div class="details">
                <div class="details-inner">
                    <h3>The latest</h3>
                    <p>{entry.progress}</p>
                </div>
            </div>
        </div>
    {/each}
</div>

