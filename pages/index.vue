<template>
  <section class="section">
    <div class="columns is-mobile">
      <card :title="`Where will you find a ${multipler} bagger today?`">
        <b-field label="Ticker">
          <b-input @keyup.native.enter="search" v-model="ticker"></b-input>
        </b-field>
        <b-button @click="search">Search</b-button>
      </card>
    </div>
    <span v-show="mkprice" class="is-size-1"> ${{ mkprice }}/share</span>

    <div>
      <b-dropdown
        v-show="selectedExpirationDate"
        v-model="selectedExpirationDate"
        aria-role="list"
      >
        <template #trigger>
          <b-button>
            {{
              selectedExpirationDate
                ? formateDate(selectedExpirationDate)
                : 'Select an Expiration Date'
            }}
          </b-button>
        </template>

        <b-dropdown-item
          v-for="(date, index) in dates"
          :key="index"
          :value="date"
          @click="expirationDateSelected(date)"
        >
          {{ formateDate(date) }}
        </b-dropdown-item>
      </b-dropdown>
    </div>
    <div>
      <b-table
        v-show="baggerData.length > 0"
        :default-sort="['percent_change_required', 'asc']"
        :loading="loading"
        :data="baggerData"
        :columns="columns"
      ></b-table>
    </div>
    <footer v-show="lastUpdated">Last Updated: {{ lastUpdated }}</footer>
  </section>
</template>

<script>
import Card from '~/components/Card'

export default {
  name: 'HomePage',

  data() {
    return {
      ticker: null,
      dates: [],
      mkprice: null,
      optionData: null,
      selectedExpirationDate: null,
      thershold: 100,
      loading: false,
      multipler: 10,
      baggerData: [],
      lastUpdated: null,
      columns: [
        {
          field: 'strike_price',
          label: 'Strike Price ($)',
          sortable: true,
          numeric: true,
        },
        {
          field: 'percent_change_required',
          label: 'Percent Change Required (%)',
          sortable: true,
          numeric: true,
        },
        {
          field: 'target_price',
          label: 'Target Price ($)',
          sortable: true,
          numeric: true,
        },
        {
          field: 'cost',
          label: 'Cost ($)',
          sortable: true,
          numeric: true,
        },
        {
          field: 'open_interest',
          label: 'Open Interest',
          sortable: true,
          numeric: true,
        },
      ],
    }
  },
  components: {
    Card,
  },
  methods: {
    async search() {
      const { ticker, dates } = this
      this.baggerData = []
      try {
        await this.fetchMarketPrice(ticker)
        await this.fetchOptionsDates(ticker)
        this.$buefy.notification.open(
          'Here are some potential 10 baggers for ' + ticker
        )
      } catch (err) {
        this.$buefy.notification.open(`Invalid Ticker`)
      }
      this.ticker = ''
    },
    async fetchMarketPrice(ticker) {
      this.mkprice = await this.$axios.$get(
        `https://cloud.iexapis.com/v1/stock/${ticker}/price?token=pk_1e940096696b46e0bb280fa59c46c05a`
      )
    },
    async fetchOptionsDates(ticker) {
      this.dates = await this.$axios.$get(
        `https://cloud.iexapis.com/v1/stock/${ticker}/options?token=pk_1e940096696b46e0bb280fa59c46c05a`
      )
      await this.fetchOptionsData(ticker, this.dates[1])
      this.selectedExpirationDate = this.dates[1]
    },
    async fetchOptionsData(ticker, date) {
      this.loading = true
      this.optionData = await this.$axios.$get(
        `https://cloud.iexapis.com/v1/stock/${ticker}/options/${date}/call?token=pk_1e940096696b46e0bb280fa59c46c05a`
      )
      this.createBaggerdata()
    },
    createBaggerdata() {
      this.baggerData = []
      this.optionData.forEach(
        ({ strikePrice, ask, expirationDate, openInterest }) => {
          let targetPrice = this.calculateTargetPrice(strikePrice, ask)
          this.baggerData.push({
            strike_price: strikePrice,
            target_price: Number(targetPrice),
            open_interest: openInterest,
            percent_change_required: Number(this.percentageChange(targetPrice)),
            cost: ask,
            open_interest: openInterest,
          })
        }
      )
      this.loading = false
    },
    calculateTargetPrice(strikePrice, optionCost) {
      return (strikePrice + optionCost * this.multipler).toFixed(2)
    },
    percentageChange(targetPrice) {
      return (((targetPrice - this.mkprice) / this.mkprice) * 100).toFixed(2)
    },
    setLastUpdated(date) {
      this.lastUpdated = date
    },
    expirationDateSelected(date) {
      this.fetchOptionsData(this.ticker, date)
    },
    formateDate(date) {
      var dateString = date
      var year = dateString.substring(0, 4)
      var month = dateString.substring(4, 6)
      var day = dateString.substring(6, 8)

      return new Date(year, month - 1, day).toLocaleDateString('en-US')
    },
  },
}
</script>
