<script setup>
// Importing necessary Vue.js features
import { ref, watch } from 'vue';

// Defining props for the component
const props = defineProps({
  masterAccount: Object // Assuming masterAccount is an object
});

// Declaring reactive variables to store master account data
const masterAccountData = ref(props.masterAccount);
const masterBalance = ref();
const masterEquity = ref();
const masterWithdrawals = ref();
const masterDeposits = ref();
const masterProfit = ref();

// Watching for changes in the masterAccount prop
watch(() => props.masterAccount, (newValue, oldValue) => {
    // Update master account data variables
    masterAccountData.value = newValue;
    masterBalance.value = newValue.balance;
    masterEquity.value = newValue.equity;
    masterWithdrawals.value = newValue.withdrawals;
    masterDeposits.value = newValue.deposits;
    masterProfit.value = newValue.profit;

    // Create or update radar chart based on the master account data
    createOrUpdateRadarChart();
  
    // Add event listener for window resize to handle responsive font size
    window.addEventListener('resize', () => {
        // Recreate or update radar chart on window resize to adjust font size
        createOrUpdateRadarChart();
    });
});

// Function to calculate responsive font size for radar chart labels
function responsiveFonts() {
  // Adjust font size based on window dimensions
  if (window.matchMedia('(min-width: 3768px) and (min-height: 1769px) and (max-height: 1769px)').matches) {
      return 18; // Font size 18px for the new media query
  } else if (window.matchMedia('(min-width: 3768px) and (min-height: 1831px)').matches) {
      return 18; // Font size 18px for the specific media query
  } else if (window.matchMedia('(min-width: 2512px) and (min-height: 1335px) and (max-height: 1335px)').matches) {
      return 18; // Font size 18px for the most specific media query
  } else if (window.matchMedia('(min-width: 2560px) and (min-height: 1263px) and (max-height: 1430px)').matches) {
      return 16; // Font size 16px for the new media query
  } else if (window.matchMedia('(min-height: 1080px)').matches) {
      return 14; // Font size 14px for the second media query
  } else if (window.matchMedia('(min-width: 1884px) and (min-height: 1001px) and (max-height: 1001px)').matches) {
      return 14; // Font size 14px for the specific media query
  } else if (window.matchMedia('(min-width: 1884px) and (min-height: 884px) and (max-height: 884px)').matches) {
      return 12; // Font size 12px for the first media query
  } else {
      return 12; // Default font size 12px
  }
}

// Function to create or update radar chart based on master account data
function createOrUpdateRadarChart() {
  // Retrieve radar chart canvas element
  const radarCtx = document.getElementById('radarChart');
  
  // Create or retrieve existing radar chart instance
  let radarChart = Chart.getChart(radarCtx);

  // If radar chart doesn't exist, create a new one
  if (!radarChart) {
    radarChart = new Chart(radarCtx, {
      type: 'radar',
      data: {
        labels: [`Balance: $ ${masterBalance.value}`, `Equity: $ ${masterEquity.value}`, `Withdrawal: $ ${masterWithdrawals.value}`, `Deposit: $ ${masterDeposits.value}`, `Profit: $ ${masterProfit.value}`],
        datasets: [{
          label: '$',
          data: [
            masterBalance.value,
            masterEquity.value,
            masterWithdrawals.value,
            masterDeposits.value,
            masterProfit.value
          ],
          borderWidth: 1,
          pointStyle: 'circle',
          pointRadius: 3
        }]
      },
      options: {
        maintainAspectRatio: false,
        scales: {
          y: {
            display: false,
            suggestedMin: 0, //0
            suggestedMax: 100,  //100
          },
          r: {
            angleLines: {
              color: '#475569' //#475569
            },
            grid: {
              color: '#475569'
            },
            display: true,
            suggestedMin: 0, //40
            suggestedMax: 100, //100
            pointLabels: {
              font: {
                size: responsiveFonts()
              },
              color: 'white'
            },
            ticks: {
              display: false
            }
          }
        },
        plugins: {
          legend: {
            display: false
          }
        },
        elements: {
          line: {
            borderWidth: 2
          }
        }
      }
    });
  } else {
    // If radar chart exists, update its data
    radarChart.data.labels = [
      `Balance: $ ${masterBalance.value}`,
      `Equity: $ ${masterEquity.value}`,
      `Withdrawal: $ ${masterWithdrawals.value}`,
      `Deposit: $ ${masterDeposits.value}`,
      `Profit: $ ${masterProfit.value}`
    ];
    radarChart.data.datasets[0].data = [
      masterBalance.value,
      masterEquity.value,
      masterWithdrawals.value,
      masterDeposits.value,
      masterProfit.value
    ];
    radarChart.update(); // Update the chart
  }
}
</script>

<template>
    <div id="box9-2-1" class="content-middle-top-1">
        <div class="dashboard-title">
            <div class="bullet-points">
                <div class="white-border">
                    <div class="blue-bracket">[ 
                        <div class="small-square"></div>
                        ]
                    </div>
                </div>
            </div>
            <div class="title-name">
                <h3>Finance Summary</h3>
            </div>
        </div>
        <div class="trader-dashboard-frame">
            <div class="chart-container">
                <div class="radar-chart">
                    <canvas id="radarChart"></canvas>
                </div>
            </div>
        </div>
    </div>
</template>