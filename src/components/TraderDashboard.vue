<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios'; // Import Axios


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

// 

const masterData = ref([]);

async function fetchData() {
    try {
        const response = await axios.get('https://member.luckyantfxasia.com/api/getMaster');
        masterData.value = response.data.metaUser;
        console.log('Master Data:', masterData.value);
    } catch (error) {
        console.error('Error fetching live data:', error);
    }
}

onMounted(() => {
    fetchData(); // Fetch data when the component is mounted
});

// Function to handle click event on master
function handleMasterClick(index) {
    // Reset background color of all masters
    masterData.value.forEach((master, idx) => {
        master.clicked = idx === index;
    });
}


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
        <!-- <div id="clickable1" v-for="(user, index) in masterData" :key="index" class="trader-dashboard-frame"> -->
            <div id="clickable1" v-for="(user, index) in masterData" :key="index" class="trader-dashboard-frame" :class="{ 'clicked': user.clicked }" @click="handleMasterClick(index)">
            <div class="pamm-master-box">
                <div class="dashboard-name-border">
                    <div class="dashboard-name">
                        <h4>{{ user.meta_user.name }}</h4> <!-- Display username or show error message -->
                    </div>
                </div>
            <div class="trader-dashboard-content">
                <div class="trader-dashboard-img-1">
                    <div class="shadow-container">
                        <div class="hexagon-border">
                            <img src="/src/assets/dashboard/dashboard-1-s.jpg" alt="">
                        </div>
                    </div>
                    
                </div>
                <div class="content-up-down">
                    <div class="add-onbox">
                        <div class="trader-dashboard-content-right">
                            <div class="trader-dashboard-content-right-name">
                                <p>Balance</p>
                                <p>Investors</p>
<!--                                <p>Total Growth</p>-->
                            </div>
                        </div>
                        <div class="trader-dashboard-content-right-data">
                            <p>$ {{ user.meta_user.balance }} </p>
                            <p>{{ user.subscriber }}</p>
<!--                            <p class="total-growth">+2,383.39%</p>-->
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
        </div>
    </div>
</template>
