<script setup>
import { ref, watch, onMounted } from 'vue';
import axios from 'axios';
import Chart from 'chart.js/auto';

const props = defineProps({
    masterAccount: Object
});

const master_meta_login = ref();
const data = ref([]);
let pieChart = null;

const fetchData = async () => {
    try {
        const response = await axios.get(`https://member.luckyantfxgroup.com/api/getMasterCurrency?meta_login=${master_meta_login.value}`);
        data.value = response.data;
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

function generateColorPalette(numColors) {
    const palette = [];
    for (let i = 0; i < numColors; i++) {
        const r = Math.floor(Math.random() * 256);
        const g = Math.floor(Math.random() * 256);
        const b = Math.floor(Math.random() * 256);
        const color = `#${r.toString(16).padStart(2, '0')}${g.toString(16).padStart(2, '0')}${b.toString(16).padStart(2, '0')}`;
        palette.push(color);
    }
    return palette;
}

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

function createPieChart(data) {
    if (pieChart) {
        pieChart.destroy();
    }

    const pieCtx = document.getElementById('pieChart');

    const labels = data.map(item => item.label);
    const percentages = data.map(item => item.percentage);

    pieChart = new Chart(pieCtx, {
        type: 'pie',
        data: {
            labels: labels,
            datasets: [{
                data: percentages,
                backgroundColor: generateColorPalette(data.length),
                borderColor: '#000000',
                borderWidth: 2
            }]
        },
        options: {
            maintainAspectRatio: false,
            layout: {
                padding: 20
            },
            responsive: true,
            plugins: {
                datalabels: {
                    color: 'white',
                    anchor: 'end',
                    align: 'end',
                    font: {
                        size: responsiveFonts()
                    },
                    formatter: (value, pieCtx) => {
                        const data = pieCtx.dataset.data;
                        const totalSum = data.reduce((accumulator, currentValue) => accumulator + currentValue, 0);
                        const percentage = value / totalSum * 100;
                        return `${labels[value.index]} ${percentage.toFixed(2)}%`;
                    }
                }
            }
        }
    });
}

onMounted(() => {
    fetchData();
});

watch(data, (newData) => {
    if (newData.length > 0) {
        createPieChart(newData);
    }
});

</script>

<template>
    <div id="box9-2-2" class="content-middle-top-2">
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
                <h3>Asset Allocation</h3>
            </div>
        </div>
        <div class="trader-dashboard-frame">
            <div class="chart-container">
                <div class="pie-chart">
                    <canvas id="pieChart"></canvas>
                </div>
            </div>
        </div>
    </div>
</template>

<!-- <script setup>

import { onMounted } from 'vue';

// const props = defineProps({
//     masterAccount: Object
// });

// // Declaring reactive variables to store master account data
// const masterAccountData = ref(props.masterAccount);



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

// Function to create pie chart
function createPieChart() {
  const pieCtx = document.getElementById('pieChart');
  
  // Create the pie chart and store it in a variable
  const pieChart = new Chart(pieCtx, {
      type: 'pie',
      data: {
        datasets: [{
          label: 'value',
          data: [
              { label: 'XAUUSD', value: 100 }
          ],
          backgroundColor:[
                '#4DD1FF'
          ],
          borderColor: [
                '#000000'
            ],
          borderWidth: 2
        }]
      },
      options: {
        maintainAspectRatio: false,
        layout: {
          padding: 20 //zoom in and out
        },
        responsive: true,
        plugins: {
            datalabels: {
              color: 'white',
              anchor: 'end',
              align: 'end',
              font: {
                size: responsiveFonts() // Set the font size based on window width
              },
              formatter: (value, pieCtx) => {
                      const data = pieCtx.dataset.data;
                      const totalSum = data.reduce((accumulator, currentValue) => accumulator + currentValue.value, 0);
                      const percentage = value.value / totalSum * 100;
                      return `${value.label} ${percentage.toFixed(2)}%`;
                  }
            }
        }
      },
      plugins: [ChartDataLabels]
  }); 
}

// Call createPieChart function when the component is mounted
onMounted(() => {
  createPieChart();
  
  // Add event listener for window resize
  window.addEventListener('resize', () => {
    // Recreate pie chart on window resize to update font size
    createPieChart();
  });
});

</script>

<template>
    <div id="box9-2-2" class="content-middle-top-2">
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
                <h3>Asset Allocation</h3>
            </div>
        </div>
        <div class="trader-dashboard-frame">
            <div class="chart-container">
                <div class="pie-chart">
                    <canvas id="pieChart"></canvas>
                </div>
            </div>
        </div>
    </div>
</template> -->
