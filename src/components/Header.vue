<template>
  <nav class="navbar navbar-expand bg-dark navbar-dark">
    <div class="navbar-header">
      <router-link to="/" class="navbar-brand">Stock Trader</router-link>
    </div>
    <div class="collapse navbar-collapse justify-content-between">
      <ul class="navbar-nav">
        <router-link to="/portfolio" activeClass="active" class="nav-link"
          >Portfolio</router-link
        >
        <router-link to="/stocks" activeClass="active" class="nav-link"
          >Stocks</router-link
        >
      </ul>
      <strong class="navbar-text">Funds: {{ funds | currency }}</strong>
      <ul class="navbar-nav">
        <li><a href="#" @click="endDay" class="nav-link">End Day</a></li>
        <li class="dropdown" @click="isDropdownOpen = !isDropdownOpen">
          <a
            href="#"
            class="dropdown-toggle nav-link"
            data-toggle="dropdown"
            role="button"
            aria-haspopup="true"
            aria-expanded="false"
            >Save & Load <span class="caret"></span
          ></a>
          <ul class="dropdown-menu" :class="{ show: isDropdownOpen }">
            <li>
              <a href="#" class="dropdown-item" @click="saveData">Save Data</a>
            </li>
            <li>
              <a href="#" class="dropdown-item" @click="loadData">Load Data</a>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
import { mapActions } from "vuex";

export default {
  data() {
    return {
      isDropdownOpen: false
    };
  },
  computed: {
    funds() {
      return this.$store.getters.funds;
    }
  },
  methods: {
    ...mapActions({
      randomizeStocks: "randomizeStocks",
      fetchData: "loadData"
    }),
    endDay() {
      this.randomizeStocks();
    },
    saveData() {
      const data = {
        funds: this.$store.getters.funds,
        stockPortfolio: this.$store.getters.stockPortfolio,
        stocks: this.$store.getters.stocks
      };
      this.$http.put("data.json", data);
    },
    loadData() {
      this.fetchData();
    }
  }
};
</script>
