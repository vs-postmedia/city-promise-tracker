<script>
    import { onMount } from 'svelte';

    // COMPONENTS
    import Tooltip from '$components/Tooltip.svelte';

    let { data = [], value = '' } = $props();

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

    $effect(() => {
        // console.log(data);
    });

    // FUNCTIONS
    function init() {
        console.log('CHART INIT!')
    }
    function addCommasToNumber(number) {
        return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }

    // LIGHTS! CAMERA! ACTION!
    onMount(init);
</script>

<div class="chart-container" bind:clientWidth={width}>
    <p>Chart goes here....</p>
</div>


<style>
    .chart-container {
        border: 2px solid var(--orange01);
        height: 200px;
        margin: 0 auto 1rem auto;
        max-width: 500px;
    }
</style>