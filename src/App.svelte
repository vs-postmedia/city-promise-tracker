<script>
    // COMPONENTS
    import { onMount } from 'svelte';
    import { csvParse } from 'd3-dsv';
    import Chart from "$components/Chart.svelte";
    import Filters from "$components/Filters.svelte";
    import Cards from "$components/Cards/Cards.svelte";
    import Select from "svelte-select"; // https://github.com/rob-balfre/svelte-select

    // DATA
    // import data from "$data/data.js";
    import { menuItems } from "$data/menu-items";
    const dataUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTdPwb69KcPAH91-rkyHoPIVHaEYARJKRiHjm4i6gTHyobfMC-mAsX07IO-ASPbOF7GnUKcgzLq0HRt/pub?gid=0&single=true&output=csv';

    // VARIABLES
    let data = $state();
    let value = $state();
    let activeCategory = $state('');
    let chartData = $derived(
        data?.filter((row) => {
            const selectedCity = typeof value === 'string' ? value : value?.value;
            return row.city?.toLowerCase() === selectedCity?.toLowerCase();
        }) ?? []
    );
    const defaultSelectValue = 'vancouver'; // menuItems[0].value;


    async function fetchData(url) {
        const resp = await fetch(url, { cache: 'no-store' });
        const data = await resp.text();

        return csvParse(data);
    }

    function updateData(nextValue) {
        if (!nextValue || !nextValue.value) return;

        value = nextValue;
        console.log(nextValue);
    }

    async function init() {
        // fetch remote data
        data = await fetchData(dataUrl);

        // get city from URL params
        const urlParams = new URLSearchParams(window.location.search);

        const urlCity = urlParams.get('city');
        value = menuItems.find(item => item.value === urlCity) ?? defaultSelectValue;
    }

    onMount(init);
</script>

<header>
    <h1>Promise tracker</h1>
    <p class="subhead">Politicians make a lot of promises on the campaign trail. Postmedia is tracking some of them.</p>
</header>

<main>
    <Select items={menuItems}
        bind:value
        change={updateData}
        placeholder="Pick a city..."
		showChevron="true"
		listOpen={false}
    />
    
    <Filters
        data={chartData}
        bind:active={activeCategory}
    />

    <Chart 
        data={chartData}
        value={value}
    />
    
    <Cards 
        data={chartData}
        active={activeCategory}
        value={value}
    />
</main>

<footer>
    <p class="note">NOTE: tk.</p>
    <p class="source">Source:  Postmedia reporting</p>
</footer>
  
<style>
    @import '$css/normalize.css';
    @import '$css/fonts.css';
    @import '$css/colors.css';
    @import '$css/app.css';

    header {
		margin-bottom: 2rem;
	}
	header > h1 {
		text-align: center;
	}
	header .subhead {
		margin: 0 auto;
		max-width: 525px;
		text-align: center;
	}

    /* COMBOBOX SELECTOR */
  	:global(.svelte-select) {
		margin: 1rem auto !important;
		max-width: 250px;
  	}
  	:global(input:focus) {
		outline: none;
  	}

	:global(
		.svelte-select .selected-item,
		.svelte-select .item,
		.svelte-select input
	) {
		font-family: 'BentonSansCond-Regular', sans;
	}
</style>
