<template>
  <Doughnut
    :key="chartKey"
    :data="data"
    :options="options"
    v-if="userCosts.length > 0"
  ></Doughnut>
  <p v-else>
    موردی موجود نیست! در صفحه ی هزینه ها، هزینه های خود را یادداشت کنید
  </p>
</template>

<script>
import {
  Chart as ChartJS,
  ArcElement,
  Tooltip,
  Legend,
} from 'chart.js'
import { Doughnut } from 'vue-chartjs'
import { computed, inject, nextTick} from 'vue'
import ToDarkMode from '../hooks/ToDarkMode'

ChartJS.register(ArcElement, Tooltip, Legend)

export default {
  components: {
    Doughnut
  },
  setup () {
    let userData = inject('userData')
    let costs = inject('costs')

    let userCosts = computed(() =>
      costs.value.filter(cost => cost[1].for == userData.value.name && cost[1].isPurchased)
    )

    let categories = ['اینترنت', 'رفت و آمد', 'تفریح', 'ماشین', 'خانه', 'دیگر']

    function calculate (category) {
      let allCosts = userCosts.value.filter(
        cost => cost[1].selectCategory == category 
      )

      let sum = 0

      allCosts.forEach(cost => (sum += cost[1].price))

      return sum
    }
    let data = computed(() => {
      nextTick(() => ({
        labels: categories,
        datasets: [
          {
            backgroundColor: [
              '#f87979',
              '#79f879',
              '#7979f8',
              '#f8e479',
              '#79e4f8',
              '#f879e4'
            ],
            data: [
              calculate('اینترنت'),
              calculate('رفت و آمد'),
              calculate('تفریح'),
              calculate('ماشین'),
              calculate('خانه'),
              calculate('دیگر')
            ]
          }
        ]
      }))

      return {
        labels: categories,
        datasets: [
          {
            backgroundColor: [
              '#f87979',
              '#79f879',
              '#7979f8',
              '#f8e479',
              '#79e4f8',
              '#f879e4'
            ],
            data: [
              calculate('اینترنت'),
              calculate('رفت و آمد'),
              calculate('تفریح'),
              calculate('ماشین'),
              calculate('خانه'),
              calculate('دیگر')
            ]
          }
        ]
      }
    })

    const options = computed(() => ({
      responsive: true
    }))

    const chartKey = ToDarkMode()

    return { data, options, userCosts, chartKey }
  }
}
</script>
