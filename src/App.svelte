<script>
  import { onMount } from 'svelte'
  const API_URL =
    'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false'

  let titles = [' ', 'COIN', 'PRICE', 'CHANGE', '24h VOLUME']
  let coins = []
  let filteredCoins = []
  let ref = null

  const loadCoins = async () => {
    const res = await fetch(API_URL)
    const data = await res.json()
    console.log(data)
    coins = data
    filteredCoins = data
  }

  loadCoins()

  const searchCoin = (value) => {
    filteredCoins = coins.filter(
      (coin) =>
        coin.name.toLowerCase().includes(value.toLowerCase()) ||
        coin.symbol.toLowerCase().includes(value.toLowerCase())
    )
  }

  onMount(() => {
    ref.focus()
  })
</script>

<!-- Aca comienza el codigo HTML -->
<h1>COIN MARKET</h1>
<div class="container">
  <div class="row">
    <!--boton de busqueda-->
    <input
      type="text"
      class="input"
      placeholder="Search Your Coin"
      on:keyup={({ target: {value} }) => searchCoin(value)}
      bind:this={ref}
    />

    <table class="table table-dark table-striped ">
      <!--aca empieza la cabezera de nuestra tabla-->
      <thead>
        <tr class="text-warning">
          <th scope="col" class="moneda"> </th>
          <th scope="col">COIN</th>
          <th scope="col">PRICE</th>
          <th scope="col">CHANGE</th>
         <th scope="col">24H</th>
        </tr>
      </thead>

      <!--aca empieza el body de nuestra tabla-->
      <tbody>
        {#each filteredCoins as coin, i}
          <tr>
            <td class="text-muted"> <img
              src={coin.image}
              alt={coin.name}
              style="width:1.5rem;"
              class="img-fluid "
            /></td>

            <!--aca empieza la columna de nombre de monedas-->
            <td class="nombre-coin">
             
              <span >
                {coin.name}
                
              </span>
              <span class="text-muted text-uppercase siglas">
                {coin.symbol}
              </span>
            </td>

            <!--aca empieza la columna precio de moneda-->
            <td class="precio-coin">
              ${coin.current_price.toLocaleString()}
            </td>
            <td
              class={coin.price_change_percentage_24h > 0
                ? 'text-info'
                : 'text-danger'}
            >
              {coin.price_change_percentage_24h} %
            </td>
            <td >
              ${coin.total_volume.toLocaleString()}
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>
<p>On January 3, 2009, an anonymous individual (or group of individuals) going by the name Satoshi Nakamoto launched Bitcoin – the first and still the biggest crypto by market cap. Bitcoin proved the feasibility of blockchain-based virtual currencies and drew the attention of investors, the tech community, and idealists who saw it as the future of money. In particular, people were impressed with Bitcoin’s features of decentralization, immutability, censorship resistance, and scarcity. 

  Although Bitcoin was the first practical public digital currency, it’s not the only crypto in existence – there are over 13,000 cryptocurrencies. These cryptos can be classified based on their use cases, utility, and many other factors. Read on to learn more about the different types of cryptocurrencies and for some ideas on how to diversify your crypto portfolio.</p>

<style>
  .input {
    margin-block: 2rem;
    background: black;
    color: whitesmoke;
    font-size: 1.2rem;
  }

  p{
    margin-inline: 8%;
    text-align: left;
    text-justify:inter-word;
    font-size: 1.3rem;
    margin-bottom: 3rem;
  }
 
  @media(max-width:800px){
    p{
      margin-inline: 0.5rem;
    }
    .siglas{
      display: none;
    }
    .moneda{
      display: none;
    }
    .text-muted{
      display: none;
    }
    .nombre-coin{
    width: 0.5rem;
  }
    .precio-coin{
    width: 0.5rem;
  }
    
  }
</style>
