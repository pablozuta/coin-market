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
<main>
  <header>
    <h1>COIN MARKET</h1>
  </header>

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

      <table class="table table-dark table-striped">
        <!--aca empieza la cabezera de nuestra tabla-->
        <thead>
          <tr class="text-warning">
            <th scope="col" class="moneda" />
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
              <td class="text-muted">
                <img
                  src={coin.image}
                  alt={coin.name}
                  style="width:1.5rem;"
                  class="img-fluid "
                /></td
              >

              <!--aca empieza la columna de nombre de monedas-->
              <td class="nombre-coin">
                <span>
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
              <td>
                ${coin.total_volume.toLocaleString()}
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>
</main>

<style>
  h1 {
    color: whitesmoke;
    letter-spacing: 0.7rem;
  }
  .input {
    margin-block: 2rem;
    background: black;
    color: whitesmoke;
    font-size: 1.2rem;
    
  }

  @media (max-width: 800px) {
    .siglas {
      display: none;
    }
    .moneda {
      display: none;
    }
    .text-muted {
      display: none;
    }
    .nombre-coin {
      width: 0.5rem;
    }
    .precio-coin {
      width: 0.5rem;
    }
    .input {
      width: 90%;
      margin-inline: auto;
    }
  }
</style>
