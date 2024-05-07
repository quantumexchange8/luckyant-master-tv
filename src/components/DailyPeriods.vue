<script setup>

import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import Chart from 'chart.js/auto';

const props = defineProps({
    masterAccount: Object
})
const dates = ref([]);
const longTrades = ref([]);
const shortTrades = ref();
const master_meta_login = ref();

const fetchData = async () => {
    try {
        const response = await axios.get(`https://member.luckyantfxasia.com/api/getMasterLatestTrades?meta_login=${master_meta_login.value}`);
        dates.value = response.data.dates
        longTrades.value = response.data.longTrades;
        shortTrades.value = response.data.shortTrades;
        // console.log('Live Masters:', shortTrades.value);
    } catch (error) {
        console.error('Error fetching live data:', error);
    }
}

watch(() => props.masterAccount, (newMasterAccount) => {
    if (newMasterAccount) {
        master_meta_login.value = newMasterAccount.login; // Update master_meta_login with the new value
        fetchData(); // Fetch data with the updated meta_login value
    }
});

// Function to calculate responsive font size
function responsiveFonts() {
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

// Gradient function for "Long Trades"
function getLongTradesGradient(ctx, chartArea) {
    const gradientBg = ctx.createLinearGradient(0, 0, 0, chartArea.bottom);

    // Start with solid color
    gradientBg.addColorStop(0, 'rgba(56, 189, 248, 1)'); // Solid color (fully opaque)

    // Gradually become transparent
    gradientBg.addColorStop(1, 'rgba(56, 189, 248, 0)'); // Fully transparent

    return gradientBg;
};

// Gradient function for "Short Trades"
function getShortTradesGradient(ctx, chartArea) {
    const gradientBg = ctx.createLinearGradient(0, 0, 0, chartArea.bottom);

    // Start with solid color
    gradientBg.addColorStop(0, 'rgba(0, 221, 225, 1)'); // Solid color (fully opaque)

    // Gradually become transparent
    gradientBg.addColorStop(1, 'rgba(0, 221, 225, 0)'); // Fully transparent

    return gradientBg;
};

// Setup data
const data = {
    labels: dates.value,
    datasets: [{
        label: 'Long Trades',
        data: longTrades.value,
        backgroundColor: function(context) {
            const chart = context.chart;
            const { ctx, chartArea } = chart;
            if (!chartArea) {
                return null;
            }
            return getLongTradesGradient(ctx, chartArea);
        },
        borderWidth: 0,
    }, {
        label: 'Short Trades',
        data: shortTrades.value, // Different data for second dataset
        backgroundColor: function(context) {
            const chart = context.chart;
            const { ctx, chartArea } = chart;
            if (!chartArea) {
                return null;
            }
            return getShortTradesGradient(ctx, chartArea);
        },
        borderWidth: 0,
    }]
};

// Configuration
const config = {
    type: 'bar',
    data: data,
    options: {
        maintainAspectRatio: false,
        plugins: {
            legend: {
                labels: {
                    boxWidth: 10,
                    boxHeight: 10,
                    font: {
                        size: responsiveFonts() // Set the font size based on window width
                    }
                },
                display: true
            }
        },
        scales: {
            x: {
                grid: {
                    drawOnChartArea: false,
                    drawTicks: false,
                    borderColor: '#94A3B8',
                },
                border: {
                    color: '#94A3B8'
                },
                ticks: {
                    font: {
                        size: responsiveFonts() // Set the font size based on window width
                    }
                }
            },
            y: {
                grid: {
                    drawOnChartArea: false,
                    drawTicks: false,
                    borderColor: '#94A3B8',
                },
                border: {
                    color: '#94A3B8'
                },
                ticks: {
                    padding: 10, // Add padding to the ticks
                    font: {
                        size: responsiveFonts() // Set the font size based on window width
                    }
                }
            },
        }
    }
}

// Call the responsiveFonts function after chart initialization
onMounted(async () => {
    // Fetch data initially
    await fetchData();

    // Initialize chart with initial data
    const multiBarChart = new Chart(
        document.getElementById('multiBarChart'),
        config
    );

    // Update font size for legend and x-axis ticks
    multiBarChart.options.plugins.legend.labels.font.size = responsiveFonts();
    multiBarChart.options.scales.x.ticks.font.size = responsiveFonts();
});

// Watch for changes in data and update the chart accordingly
watch([longTrades, shortTrades], () => {
    const multiBarChart = Chart.getChart('multiBarChart');
    if (multiBarChart) {
        multiBarChart.data.labels = dates.value;
        multiBarChart.data.datasets[0].data = longTrades.value;
        multiBarChart.data.datasets[1].data = shortTrades.value;
        multiBarChart.update();
    }
});

// Call the responsiveFonts function after window resize
window.addEventListener('resize', function() {
    const fontSize = responsiveFonts();
    const multiBarChart = Chart.getChart('multiBarChart');

    // Update font size for legend and x-axis ticks
    multiBarChart.options.plugins.legend.labels.font.size = fontSize;
    multiBarChart.options.scales.x.ticks.font.size = fontSize;

    multiBarChart.update();
});

// Initial call to responsiveFonts function
responsiveFonts();

</script>

<template>
    <div id="box12" class="content-long-short">
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
                <h3>Latest Longs vs. Shorts</h3>
            </div>
        </div>
        <div class="trader-dashboard-frame" >
            <!-- style="height: 87%;" -->
            <div class="bar-chart-3D">
                <canvas id="multiBarChart" style="height: 400px; width: 290px;"></canvas>
            </div>
        </div>
    </div>
</template>