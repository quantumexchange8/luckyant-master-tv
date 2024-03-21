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

const tradeHistories = ref([])
const accountId = ref()

async function fetchData() {
    try {
        const response = await axios.get(`https://www.myfxbook.com/api/get-open-trades.json?session=4gcHQQj80BSwyYjywWCy3636342&id=${accountId.value}`);
        
        tradeHistories.value = response.data; // Set the metaLogin value upon successful login
        // console.log('Trade History: ', tradeHistories.value)
    } catch (error) {
        console.error('Error fetching open trade data:', error);
        console.log('Login Status: Failed');    
    }
}

// Fetch data initially and whenever accountId changes
// onMounted(fetchData);

// Also watch for changes in tradeHistories
watch(() => props.masterAccount, (newMasterAccount) => {
    if (props.masterAccount) {
        accountId.value = newMasterAccount.id
        fetchData();
    }
});

// // Fetch data with the provided meta_login value
// fetchData('10773318');

// // Calculate duration of close - open date and time 
// // Function to calculate duration in seconds
// const calculateDuration = (openTime, closeTime) => {
//     const openDate = new Date(openTime);
//     const closeDate = new Date(closeTime);

//     const durationMilliseconds = closeDate - openDate;
//     const durationSeconds = Math.floor(durationMilliseconds / 1000);

//     return durationSeconds;
// };

// // Set interval to update data every second
// setInterval(() => {
//     fetchData('10773318');
// }, 1000);

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
                            Pips
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Net Profit (USD)
                        </th>
                        <!-- <th scope="col" class="px-6 py-3">
                            Gain
                        </th> -->
                    </tr>
                </thead>
                <tbody v-if="tradeHistories.openTrades && tradeHistories.openTrades.length > 0">
                    <!-- <tr v-for="tradeHistory in tradeHistories" :key="tradeHistory.id" class="table-content-bckg"> -->
                        <tr v-for="(tradeHistory, index) in tradeHistories.openTrades.slice(0, 5)" :key="index" :class="{ 'table-content-bckg': index % 2 === 0, 'table-content': index % 2 !== 0 }">
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    <!-- 01.01.2024 -->
                                    <!-- {{ tradeHistory.openTime }} -->
                                    {{ tradeHistory.openTime.split(' ')[0] }}
                                </div>
                                <div class="table-content-time">
                                    <!-- 11:37:48 -->
                                    {{ tradeHistory.openTime.split(' ')[1] }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            <!-- GBPUSD -->
                            {{ tradeHistory.symbol }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- Sell -->
                            {{ tradeHistory.action }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 500.00 -->
                            {{ tradeHistory.sizing.value }}
                        </td>
                        <td class="px-6 py-4">
                            <!-- 1.29506 -->
                            {{ tradeHistory.openPrice }}
                        </td>
                        <td class="px-6 py-4">
                            <span :style="{ color: tradeHistory.pips > 0 ? '#06F7A1' : (tradeHistory.pips < 0 ? '#FF483D' : 'inherit') }">
                                {{ tradeHistory.pips }}
                            </span>
                        </td>
                        <td class="px-6 py-4">
                            <!-- -25,000.00 -->
                            <!-- {{ tradeHistory.profit }} -->
                            <span :style="{ color: tradeHistory.profit > 0 ? '#06F7A1' : (tradeHistory.profit < 0 ? '#FF483D' : 'inherit') }">
                                {{ tradeHistory.profit }}
                            </span>
                        </td>
                        <!-- <td class="px-6 py-4">
                        </td> -->
                    </tr>
                    <!-- <tr class="table-content">
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:59:16
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:58:54
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            GBPUSD
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            Sell
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            500.00
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29506
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29556
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            -5.0
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            14,372.32
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            38s
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            13.23%
                            {{ loginStatus }}
                        </td>
                    </tr>
                    <tr class="table-content-bckg">
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:50:12
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:52:00
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            GBPUSD
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            Sell
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            500.00
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29506
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29556
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            -5.0
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            75,373.83
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            108s
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            67.27%
                            {{ loginStatus }}
                        </td>
                    </tr>
                    <tr class="table-content">
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:38:03
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:38:56
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            GBPUSD
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            Sell
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            500.00
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29506
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29556
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            -5.0
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            -2,378.99
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            53s
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            -12.36%
                            {{ loginStatus }}
                        </td>
                    </tr>
                    <tr class="table-content-bckg">
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:00:38
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            <div class="table-content-date-time">
                                <div class="table-content-date">
                                    01.01.2024
                                    {{ loginStatus }}
                                </div>
                                <div class="table-content-time">
                                    10:00:52
                                    {{ loginStatus }}
                                </div>
                            </div>
                        </td>
                        <td class="px-6 py-4">
                            GBPUSD
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            Sell
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            500.00
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29506
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            1.29556
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            -5.0
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            93,383.37
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            14s
                            {{ loginStatus }}
                        </td>
                        <td class="px-6 py-4">
                            26.91%
                            {{ loginStatus }}
                        </td>
                    </tr> -->
                </tbody>
            </table>
        </div>
</div>
</template>