<script>
	let coins = [];
    let filterCoins = [];
	let titles = [
        "#",
        "Coin",
        " ",
        "Price",
        "Price Change",
        "Volums"
      ];

	const loadCoin =  async () => {
		const res =	await fetch('http://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false');
		const data = await res.json();
		coins = data;
		filterCoins = data;
	}

	const search = (value) =>{
      filterCoins = coins.filter((coin) => coin.name.toLowerCase().includes(value.toLowerCase()));
    }

	loadCoin();
</script>

<main>
	<div class="container">
		<div class="row">
		  <h1>COIN</h1>
		  <input type="text" class="form-control bg-dark text-light rounded-0 border-0 my-4" placeholder="Search Coin" 
		  on:keyup={({target: {value}}) => search(value)}>
		  <table class="table table-dark">
			<thead>
				<tr>
					{#each titles as title}	
						<th>{ title }</th>
					{/each }
				</tr>
			</thead>
			<tbody>
			{#each filterCoins  as coin, index}
				<tr>
					<td class="text-muted"> {index + 1} </td>
					<td> 
						<img src={coin.image} style="width:2rem" class="me-2" alt="">
						<span>
							{ coin.name }
						</span>
					</td>
					<td> <span class="ms-2 text-uppercase text-muted">{ coin.symbol }</span></td>
					<td> ${ coin.current_price } </td>
					<td class={coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'}> 
						{coin.price_change_percentage_24h}%
					</td>
					<td> ${coin.total_volume} </td>
				</tr>
			{/each}
			 
			</tbody>
		  </table>
		</div>
	  </div>
</main>

<style>

</style>