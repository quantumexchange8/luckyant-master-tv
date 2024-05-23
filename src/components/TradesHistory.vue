<script setup>

import { ref, onMounted, watch } from 'vue';
import axios from 'axios';

function applyColorToProfitAndGain() {
    const TDsProfit = document.querySelectorAll('td:nth-child(9)'); // Select the 9th column (Profit)
    const TDsGain = document.querySelectorAll('td:nth-child(11)'); // Select the 11th column (Gain)

    // Apply color for Profit
    for (let i = 0; i < TDsProfit.length; i++) {
        const temp = TDsProfit[i];
        const profit = parseFloat(temp.textContent.replace(/,/g, '')); // Remove commas and parse as float
        if (profit > 0) {
            temp.classList.add("positive");
        } else if (profit < 0) {
            temp.classList.add("negative");
        }
    }

    // Apply color for Gain
    for (let j = 0; j < TDsGain.length; j++) {
        const tempGain = TDsGain[j];
        const gain = parseFloat(tempGain.textContent.replace(/,/g, '')); // Remove commas and parse as float
        if (gain > 0) {
            tempGain.classList.add("positive");
        } else if (gain < 0) {
            tempGain.classList.add("negative");
        }
    }
}

onMounted(() => {
    applyColorToProfitAndGain();
});


const props = defineProps({
    masterAccount: Object
})

const openTrades = ref([])
const master_meta_login = ref();

async function fetchData() {
    try {
        const response = await axios.get(`https://member.luckyantfxasia.com/api/getMasterOpenTrade?meta_login=${master_meta_login.value}`);
        
        openTrades.value = response.data.openTrade; // Set the metaLogin value upon successful login
        // console.log('Open Trade: ', openTrades.value)
    } catch (error) {
        console.error('Error fetching open trade data:', error);
        console.log('Login Status: Failed');    
    }
}


// Also watch for changes in openTrades
watch(() => props.masterAccount, (newMasterAccount) => {
    if (newMasterAccount) {
        master_meta_login.value = newMasterAccount.login; // Update master_meta_login with the new value
        fetchData(); // Fetch data with the updated meta_login value
    }
});

// // Fetch data with the provided meta_login value
// fetchData('10773318');

// // Calculate duration of close - open date and time 
// // Function to calculate duration in seconds
// const calculateDuration = (time_open, closeTime) => {
//     const openDate = new Date(time_open);
//     const closeDate = new Date(closeTime);

//     const durationMilliseconds = closeDate - openDate;
//     const durationSeconds = Math.floor(durationMilliseconds / 1000);

//     return durationSeconds;
// };

// Set interval to update data every 5 second
setInterval(() => {
    fetchData();
}, 5000);

</script>

<template>
    <div id="box8" class="content-bottom">
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
                <h3>Open Trades</h3>
                <!-- - {{ masterAccount ? masterAccount.id : '' }} -->
            </div>
        </div>
        <div class="information-content">
            <table class="w-full">
                <thead class="table-header">
                    <tr>
                        <th scope="col" class="px-6 py-3">
                            Open Date
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Ticket
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Symbol
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Action
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Lots
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Open Price
                        </th>
                        <th scope="col" class="px-6 py-3">
                            SL
                        </th>
                        <th scope="col" class="px-6 py-3">
                            TP
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Current Price
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Swap
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Profit
                        </th>
                        <!-- <th scope="col" class="px-6 py-3">
                            Pips
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Net Profit (USD)
                        </th> -->
                        <!-- <th scope="col" class="px-6 py-3">
                            Gain
                        </th> -->
                    </tr>
                </thead>
                <tbody v-if="openTrades && openTrades.length > 0">
                    <!-- <tr v-for="openTrade in openTrades" :key="openTrade.id" class="table-content-bckg"> -->
                        <tr v-for="(openTrade, index) in openTrades.slice(0, 5)" :key="index" :class="{ 'table-content-bckg': index % 2 === 0, 'table-content': index % 2 !== 0 }">
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    <!-- 01.01.2024 -->
                                    <!-- {{ openTrade.time_open }} -->
                                    {{ openTrade.timeCreated.split(' ')[0] }}
                                </div>
                                <div class="table-content-time">
                                    <!-- 11:37:48 -->
                                    {{ openTrade.timeCreated.split(' ')[1] }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            <!-- GBPUSD -->
                            {{ openTrade.position }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- GBPUSD -->
                            {{ openTrade.symbol }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- Sell -->
                            {{ openTrade.action }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 500.00 -->
                            {{ openTrade.volume }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 1.29506 -->
                            {{ openTrade.avgOpenPrice }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 1.29506 -->
                            {{ openTrade.priceSL.toFixed(2) }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 1.29506 -->
                            {{ openTrade.priceTP.toFixed(2) }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 1.29506 -->
                            {{ openTrade.priceCurrent }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 1.29506 -->
                            {{ openTrade.swap.toFixed(2) }}
                        </td>
                        <td class="px-6 py-4" >
                            <!-- 1.29506 -->
                            <p :class="{
                            'font-green-color': openTrade.profit > 0,
                            'font-red-color': openTrade.profit < 0
                        }">
                            {{ openTrade.profit.toFixed(2) }}
                        </p>
                        </td>
                        <!-- <td class="px-6 py-4">
                            <span :style="{ color: openTrade.pips > 0 ? '#06F7A1' : (openTrade.pips < 0 ? '#FF483D' : 'inherit') }">
                                {{ openTrade.pips }}
                            </span>
                        </td> -->
                        <!-- <td class="px-6 py-4"> -->
                            <!-- -25,000.00 -->
                            <!-- {{ openTrade.profit }} -->
                            <!-- <span :style="{ color: openTrade.profit > 0 ? '#06F7A1' : (openTrade.profit < 0 ? '#FF483D' : 'inherit') }">
                                {{ openTrade.profit }}
                            </span>
                        </td> -->
                        <!-- <td class="px-6 py-4">
                        </td> -->
                    </tr>
                </tbody>
            </table>
        </div>
</div>
</template>