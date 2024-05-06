<script setup>
import { ref, onMounted, watch, defineProps } from 'vue';
import axios from 'axios';
import Chart from 'chart.js/auto';

// Function to calculate responsive font size
function responsiveFonts() {
  // Your responsive font size logic here
}

const props = defineProps({
    masterAccount: Object
})
const dates = ref([]);
const growth = ref([]);
const master_meta_login = ref();
let lineChart = null;

const fetchData = async () => {
    try {
        const response = await axios.get(`http://127.0.0.1:8000/api/getMasterGrowth?meta_login=${master_meta_login.value}`);
        dates.value = response.data.interval_dates;
        growth.value = response.data.trade_profit_pct;
    } catch (error) {
        console.error('Error fetching live data:', error);
    }
};

watch(() => props.masterAccount, (newMasterAccount) => {
    if (newMasterAccount) {
        master_meta_login.value = newMasterAccount.login;
        fetchData();
    }
});

// Function to create chart configuration
function createChartConfig(ctx, data, fontSize, dates) {
    return {
        type: 'line',
        data: {
            labels: dates.value,
            datasets: data.datasets.map(dataset => ({
                ...dataset,
                borderColor: 'rgba(56, 189, 248, 1)',
                backgroundColor: 'rgba(56, 189, 248, 0.2)',
            }))
        },
        options: {
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    display: false
                }
            },
            scales: {
                x: {
                    grid: {
                        drawOnChartArea: false,
                        drawTicks: false
                    },
                    border: {
                        color: '#94A3B8'
                    },
                    ticks: {
                        autoSkip: false,
                        maxRotation: 30,
                        minRotation: 30,
                        font: {
                            size: fontSize
                        }
                    }
                },
                y: {
                    beginAtZero: true,
                    border: {
                        dash: [1, 4]
                    },
                    grid: {
                        color: '#94A3B8',
                    },
                    min: 0,
                    max: 1000,
                    ticks: {
                        callback: function (value, index, values) {
                            return value + '%';
                        },
                        stepSize: 100,
                        font: {
                            size: fontSize
                        }
                    }
                }
            }
        }
    };
}

// Function to update font sizes in the chart
function updateFontSizes(chart, fontSize) {
    chart.options.scales.x.ticks.font.size = fontSize;
    chart.options.scales.y.ticks.font.size = fontSize;
    chart.update();
}

// Function to initialize chart
function initChart(chartId, config) {
    if (lineChart) {
        lineChart.destroy(); // Destroy the existing chart if it exists
    }
    return new Chart(chartId, config);
}

// Call the responsiveFonts function after chart initialization
watch(dates, () => {
    if (dates.value.length > 0) {
        const fontSize = responsiveFonts();
        const chartContext = document.getElementById('lineChart');

        const lineChartData = {
            labels: dates.value,
            datasets: [{
                label: 'Line Chart Data',
                data: growth.value, // Use the fetched data here
                borderWidth: 2,
                fill: true,
                pointRadius: 0
            }]
        };

        lineChart = initChart(
            chartContext,
            createChartConfig(chartContext, lineChartData, fontSize, dates)
        );
        updateFontSizes(lineChart, fontSize);
    }
});

watch(growth, (newGrowth) => {
    if (newGrowth && newGrowth.length > 0) {
        const maxGrowth = Math.max(...newGrowth); // Find the maximum growth value
        let maxTick;

        if (maxGrowth >= 0) {
            // All positive growth values
            maxTick = Math.ceil(maxGrowth / 100) * 100;
        } else {
            // No need to handle negative growth values
            maxTick = 0;
        }

        const yAxes = lineChart.options.scales.y;

        // Update y-axis configuration
        yAxes.max = maxTick;
        yAxes.min = 0; // Minimum value is always 0 for positive growth values
        yAxes.ticks.stepSize = maxTick / 10; // Adjust step size if needed

        lineChart.update(); // Update the chart
    }
});

// Call the responsiveFonts function after window resize
window.addEventListener('resize', function() {
    const fontSize = responsiveFonts();
    if (lineChart) {
        updateFontSizes(lineChart, fontSize);
    }
});
</script>

<template>
    <div id="box9-1-1" class="content-middle-top">
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
                    <h3>Chart</h3>
                </div>
        </div>
        <div class="clickable-label-chart">
            <div class="clickable-label-chart-part-1">
                <!-- First Label -->
                <div class="label-shape clickable-shape"></div>
            <div class="wrapper">
                <div id="wrapper1" class="clickable-label-chart-rectangle-with-triangle-1">
                    <!-- <div class="content-wrapper"> -->
                    <div class="clickable-label-chart-rectangle-with-trapezoid-1"></div>
                    <div class="clickable-label-chart-name">
                    <p>Growth</p>
                    </div>
                    <!-- </div> -->
                </div> 
            </div>
            
        
            <!-- <div class="label-shape clickable-shape"></div>
            <div class="wrapper">
                <div id="wrapper2" class="clickable-label-chart-rectangle-with-trapezoid-2">
                    <div class="clickable-label-chart-name">
                        <p>Balance</p>
                    </div>
                </div>
            </div>
            
            <div class="label-shape clickable-shape"></div>
            <div class="wrapper">
                <div id="wrapper3" class="clickable-label-chart-rectangle-with-trapezoid-3">
                    <div class="clickable-label-chart-name">
                        <p>Equity</p>
                    </div>
                </div>
            </div>
        </div>
        <div class="clickable-label-chart-part-2">
            <div class="label-shape clickable-shape"></div>
                <div class="wrapper">
                    <div id="wrapper4" class="clickable-label-chart-rectangle-with-triangle-4">
                        <div class="clickable-label-chart-rectangle-with-trapezoid-4"></div>
                        <div class="clickable-label-chart-name">
                        <p>Drawdown</p>
                        </div>
                    </div>
                </div> -->
        </div>
        </div>
        <div class="line-chart">
            <canvas id="lineChart" style="height: 300px; width: 760px;"></canvas>
        </div>
</div>
</template>