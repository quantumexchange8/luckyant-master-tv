<script setup>
import { ref, onMounted, defineEmits } from 'vue';
import axios from 'axios'; // Import Axios
import GeneralInformation from './GeneralInformation.vue';

// Function to apply color to total growth
function applyColorToTotalGrowth() {
    const TDs = document.querySelectorAll('.trader-dashboard-content-right-data > p:nth-child(3)'); 
    // Select the 3rd paragraph in each .trader-dashboard-content-right-data
    for (let i = 0; i < TDs.length; i++) {
        const temp = TDs[i];
        const growth = parseFloat(temp.textContent.replace(/,/g, '')); 
        // Remove commas and parse as float
        if (growth > 0) {
            temp.classList.add("positive-growth");
        } else if (growth < 0) {
            temp.classList.add("negative-growth");
        }
    }
}

onMounted(() => {
    applyColorToTotalGrowth();
});

// // Function to initialize click event
// function initializeClickEvent() {
//     // Select all elements with IDs starting with "clickable"
//     var elements = document.querySelectorAll("[id^='clickable']");
    
//     // Loop through each element
//     elements.forEach(function(element) {
//         // Add a click event listener to each element
//         element.addEventListener("click", function() {
//             // Remove the "clicked" class from all elements
//             elements.forEach(function(el) {
//                 el.classList.remove("clicked");
//             });
//             // Add the "clicked" class to the clicked element
//             element.classList.add("clicked");
//         });
//     });
// }

// // Call the initializeClickEvent function when the DOM content is loaded
// document.addEventListener("DOMContentLoaded", initializeClickEvent);

// Define the events your component emits
const emits = defineEmits(['account-clicked']);

// Your existing code

const masterData = ref([
  { name: 'Bullion AI-Trading', clicked: false, id: 1 },
  { name: 'Titan X Robot', clicked: false, id: 2 }
]);

function handleMasterClick(index) {
  masterData.value.forEach((master, idx) => {
    master.clicked = idx === index;
  });
  // Emit the 'account-clicked' event with the selected account ID
  const selectedAccountId = masterData.value[index].id;
  emits('account-clicked', selectedAccountId);
  
  // Log the clicked account ID to the console
  console.log('Clicked account ID:', selectedAccountId);
}



// async function fetchData() {
//     try {
//         const response = await axios.get('https://member.luckyantfxasia.com/api/getMaster');
//         masterData.value = response.data.metaUser;
//         console.log('Master Data:', masterData.value);
//     } catch (error) {
//         console.error('Error fetching live data:', error);
//     }
// }

// onMounted(() => {
//     fetchData(); // Fetch data when the component is mounted
// });

// // Function to handle click event on master
// function handleMasterClick(index) {
//     // Reset background color of all masters
//     masterData.value.forEach((master, idx) => {
//         master.clicked = idx === index;
//     });
// }


</script>

<template>
    <div id="box4" class="trader-dashboard">
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
                <h3>Masters</h3>
            </div>
        </div>
        <!-- <div class="trader-dashboard-frame"> -->
        <div class="trader-dashboard-frame"
        :class="{ 'clicked': masterData[0].clicked }"
        @click="handleMasterClick(0)">
        <div class="pamm-master-box">
                <div class="dashboard-name-border">
                    <div class="dashboard-name">
                        <h4>Bullion AI-Trading</h4> 
                    </div>
                </div>
            <div class="trader-dashboard-content">
                <div class="trader-dashboard-img-1">
                    <div class="shadow-container">
                        <div class="hexagon-border">
                            <img src="/src/assets/dashboard/icon-trans.png" alt="">
                        </div>
                    </div>
                    
                </div>
                <div class="content-up-down">
                    <div class="add-onbox">
                        <div class="trader-dashboard-content-right">
                            <div class="trader-dashboard-content-right-name">
                                <p>Profit</p>
                                <p>Max Drawdown</p>
                            </div>
                        </div>
                        <div class="trader-dashboard-content-right-data">
                            <p>$ 124,823.68 </p>
                            <p>7.13%</p>

                        </div>
                    </div>
                    <div class="bar-chart-data">
                        <div class="bar-chart">
                            <div id="bar-chart-container">
                                <img id="svg-img" src="/src/assets/svg/barchart.svg" alt="">
                            </div>
                        </div>
                        <div class="bar-chart-profitability">
                            <p>Gain</p>
                            <p class="font-green-color">941.83%</p>
                        </div>
                    </div>
                </div>
            </div>
            </div>
        </div>
            <br>
            <!-- <div class="trader-dashboard-frame"> -->
            <div class="trader-dashboard-frame"
        :class="{ 'clicked': masterData[1].clicked }"
        @click="handleMasterClick(1)">
            <div class="pamm-master-box">
                <div class="dashboard-name-border">
                    <div class="dashboard-name">
                        <h4>Titan X Robot</h4> 
                    </div>
                </div>
            <div class="trader-dashboard-content">
                <div class="trader-dashboard-img-1">
                    <div class="shadow-container">
                        <div class="hexagon-border">
                            <img src="/src/assets/dashboard/icon-trans.png" alt="">
                        </div>
                    </div>
                    
                </div>
                <div class="content-up-down">
                    <div class="add-onbox">
                        <div class="trader-dashboard-content-right">
                            <div class="trader-dashboard-content-right-name">
                                <p>Profit</p>
                                <p>Max Drawdown</p>
                            </div>
                        </div>
                        <div class="trader-dashboard-content-right-data">
                            <p>$ 22,794.95 </p>
                            <p>4.37%</p>

                        </div>
                    </div>
                    <div class="bar-chart-data">
                        <div class="bar-chart">
                            <div id="bar-chart-container">
                                <img id="svg-img" src="/src/assets/svg/barchart.svg" alt="">
                            </div>
                        </div>
                        <div class="bar-chart-profitability">
                            <p>Gain</p>
                            <p class="font-green-color">54.10%</p>
                        </div>
                    </div>
                </div>
            </div>
            </div>
        </div>
        <!-- <div id="clickable1" v-for="(user, index) in masterData" :key="index" class="trader-dashboard-frame"> -->
            <!-- <div id="clickable1" v-for="(user, index) in masterData" :key="index" class="trader-dashboard-frame" :class="{ 'clicked': user.clicked }" @click="handleMasterClick(index)">
            <div class="pamm-master-box">
                <div class="dashboard-name-border">
                    <div class="dashboard-name">
                        <h4>{{ user.meta_user.name }}</h4> 
                    </div>
                </div>
            <div class="trader-dashboard-content">
                <div class="trader-dashboard-img-1">
                    <div class="shadow-container">
                        <div class="hexagon-border">
                            <img src="/src/assets/dashboard/icon-trans.png" alt="">
                        </div>
                    </div>
                    
                </div>
                <div class="content-up-down">
                    <div class="add-onbox">
                        <div class="trader-dashboard-content-right">
                            <div class="trader-dashboard-content-right-name">
                                <p>Balance</p>
                                <p>Investors</p>
                            </div>
                        </div>
                        <div class="trader-dashboard-content-right-data">
                            <p>$ {{ user.meta_user.balance }} </p>
                            <p>{{ user.subscriber }}</p>

                        </div>
                    </div>
                    <div class="bar-chart-data">
                        <div class="bar-chart">
                            <div id="bar-chart-container">
                                <img id="svg-img" src="/src/assets/svg/barchart.svg" alt="">
                            </div>
                        </div>
                        <div class="bar-chart-profitability">
                            <p>Profitability</p>
                            <p class="font-green-color">96.83%</p>
                        </div>
                    </div>
                </div>
            </div>
            </div>
        </div> -->
    </div>
</template>
