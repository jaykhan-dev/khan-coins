<template>
  <div class="flex justify-center bg-gray-900 text-white">
    <div class="lg:w-3/4">
      <h1 class="text-8xl font-bold text-center mt-20">Khan Coins</h1>
      <input
        type="text"
        class="w-full bg-gray-900 p-2 border rounded-lg my-20 text-center"
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />
      <table class="w-full text-left">
        <thead>
          <tr class="">
            <th
              v-for="title in titles"
              :key="title"
              class="text-green-600 font-bold border-b border-white"
            >
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody class="w-full">
          <tr
            v-for="(coin, index) in filteredCoins"
            :key="coin.id"
            class="hover:bg-gray-800 border-b border-opacity-20 border-white"
          >
            <td class="py-2">{{ index + 1 }}</td>
            <td class="flex py-2">
              <img :src="coin.image" style="width: 2rem" alt="" class="" />
              <span class="mx-4">{{ coin.name }}</span>
              <span class="mx-2 uppercase text-sm font-bold text-gray-500">{{
                coin.symbol
              }}</span>
            </td>
            <td>{{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }}
            </td>
            <td>{{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24 Volume"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=cad&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    console.log(data);
    this.coins = data;
    this.filteredCoins = data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style></style>
