<script>
	import { createEventDispatcher } from 'svelte';
	export let countries;	
	
	const dispatch = createEventDispatcher();
	let searchTerm = "";

	$: expand = false;
	const filterBySelected = c => c.selectedIndex >= 0;
	const filterBySearch = c => c.name.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1;
	$: filteredList = searchTerm.length > 0 ? countries.filter(filterBySearch) : expand ? countries : countries.filter(filterBySelected);		

	function diffPercent(country){
		const last = country.data[country.data.length-1]; 
		const penultimate = country.data[country.data.length-2];
		return Math.round(((last - penultimate)/penultimate) * 100);
	}
	function toggleCountry(country){
		dispatch('toggleCountry', {country: country});
		searchTerm = "";
	}
	function toggleHighlight(country){
		dispatch('toggleHighlight', {country: country});
	}
	function toggleZoom(country){
		dispatch('toggleZoom',{country:country});
	}
</script>
<p class='input-container'>
 	<i class='material-icons'>search</i>
 	<input bind:value={searchTerm} />
 	<button on:click={() => expand = expand ? false:true}>
 		<i class='material-icons'>{expand ? 'expand_less' : 'expand_more'}</i>
 	</button>
</p>
<ul>
	{#each filteredList as country}
		<li  style='background-color: {country.color}' >
			<span class='label'>{country.name}</span>
			<span class='controls'>
				{#if country.selectedIndex >= 0}
					<button style='color: {country.bgColor}' on:click={toggleHighlight(country)}>
						{#if country.order === 0}
							<i class="material-icons">star</i>
						{:else}
							<i class="material-icons">star_border</i>
						{/if}
					</button>
				{/if}
				<button style='color: {country.bgColor}' on:click={toggleCountry(country)}>
					{#if country.selectedIndex >= 0}
						<i class="material-icons">clear</i>
					{:else}
						<i class="material-icons">visibility</i>
					{/if}
				</button>

			</span>
			<!-- <span class='data-point alt'>{diffPercent(country)} %</span>			
			<span class='data-point'>{new Intl.NumberFormat().format(country.data[country.data.length-1])}</span> -->
		</li>
	{/each}
</ul>
<style>
	p{
		text-align: center;
	}
	ul{
		margin:10px 5px;
		padding:0 20px;
	}
	li{
		display:flex;
		list-style: none;
		margin-bottom:5px;
		text-align: center;
		height:40px;
		line-height:40px;
		padding:0 0 0 10px;
		width:100%;
		justify-content: flex-end;
		cursor:pointer;
		color:white;
		font-weight: bold;
		background-color: #555

	}
	li .label{
		flex-grow: 1;
		text-align:left;
	}
	li .data-point{
		background-color:rgba(100,100,100,.5);
		color:white;
		width:20%;
		font-weight:bold;
	}
	li .data-point.alt{
		background-color: rgba(200,200,200,.5);
	}
	li .controls{		
		margin:0 10px 0 0;
	}
	li .controls button{
		background-color: transparent;
		color:white;
		line-height:38px;
		text-shadow:1px 1px 3px #222;
	}
	li .controls button i{
		line-height:38px;
	}
	
	.input-container{
		padding:5px 15px 5px 25px;
		display: flex;
	}

	.input-container input{
		padding:10px 8px;
		box-sizing: border-box;
		font-size:14px;
		color:white;
		flex:1;		
		border: none;
		background-color:#888;
	}
	.input-container i{
		padding:5px 9px 0;
		background-color:#ddd;
	}
	.input-container button{
		line-height: 22px;
		padding:0;
	}

	.input-container button i{
		padding-bottom:7px!important;
	}
</style>