<script>
  import { onMount } from 'svelte'
  const API_URL =
    'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false'

  let titles = ['#', 'Coin', 'Price', 'Price Change', '24h Volume']
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
      on:keyup={({ target: { value } }) => searchCoin(value)}
      bind:this={ref}
    />

    <table class="table table-dark">
      <!--aca empieza la cabezera de nuestra tabla-->
      <thead>
        <tr>
          {#each titles as title}
            <th>{title}</th>
          {/each}
        </tr>
      </thead>

      <!--aca empieza el body de nuestra tabla-->
      <tbody>
        {#each filteredCoins as coin, i}
          <tr>
            <td class="text-muted">{i + 1}</td>

            <td>
              <img
                src={coin.image}
                alt={coin.name}
                style="width:2rem ;"
                class="img-fluid me-2"
              />
              <span>
                {coin.name}
              </span>
              <span class="text-muted text-uppercase ms-2">
                {coin.symbol}
              </span>
            </td>

            <td>
              ${coin.current_price.toLocaleString()}
            </td>
            <td
              class={coin.price_change_percentage_24h > 0
                ? 'text-success'
                : 'text-danger'}
            >
              {coin.price_change_percentage_24h} %
            </td>
            <td>
              ${coin.total_volume.toLocaleString()}
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>

<style>
  .input {
    margin-block: 2rem;
    background: black;
    color: deeppink;
    font-size: 1.2rem;
  }
</style>
