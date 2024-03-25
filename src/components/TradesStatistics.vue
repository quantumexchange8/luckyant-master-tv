<script setup>

import axios from "axios";
// import { onMounted, ref, defineProps } from "vue";

// const metaInformation = ref(null); // Data from the first API
// const openTrades = ref([]); // Data from the second API
// const accountData = ref([]); // Data from the second API
// const totalTrades = ref(0);
// const durationInMinutes = ref([]);
// const session = ref(''); // Your session value here
// const email = ref('official@luckyantfxasia.com');
// const password = ref('72l3SGK=f;8V'); // Your original password

const props = defineProps({
    masterAccount: Object
})


// const props = defineProps({
//   selectedAccountId: Number // Change the prop type to Number
// });

// const fetchAccountData = async () => {
//     try {
//         const response = await axios.get(`https://www.myfxbook.com/api/get-my-accounts.json?session=4gcHQQj80BSwyYjywWCy3636342&id=${props.accountId}`);
//         accountData.value = response.data.accounts[0];
//     } catch (error) {
//         console.error('Error fetching account data:', error);
//     }
// }


// this how to get the data? different api
// const fetchOpenTrades = async () => {
//     try {
//         const response = await axios.get(`https://www.myfxbook.com/api/get-open-trades.json?session=4gcHQQj80BSwyYjywWCy3636342&id=${props.accountId}`);
//         openTrades.value = response.data.openTrades;
//     } catch (error) {
//         console.error('Error fetching open trades:', error);
//     }
// };

// const fetchData = async () => {
//     try {
//         const response = await axios.get(`https://www.myfxbook.com/api/get-my-accounts.json?session=4gcHQQj80BSwyYjywWCy3636342&id=${props.accountId}`);
//         metaInformation.value = response.data.accounts[0];
//     } catch (error) {
//         console.error('Error fetching live data:', error);
//     }
// }

// onMounted(async () => {
//     // await fetchData(); // Fetch data when the component is mounted
//     await fetchOpenTrades(); // Fetch open trades data after fetching account data
//     // await fetchAccountData();
// });

</script>

<template>
    <div class="content-right-down">
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
                <h3>TRADES STATISTICS</h3>
            </div>
        </div>
        <div class="trader-dashboard-frame">
            <div class="information-content">
                <div class="information-content-1">
                    <div class="information-part-1">
                        <p>Total Trades</p>
                        <p>Profitability</p>
                        <p>Lots</p>
                        <p>Pips</p>
                        <p>Average Profit</p>
                        <p>Average Loss</p>
                        <p>Average Holding Duration</p>
                        <p>Latest Trade</p>
                    </div>
                    <div class="information-data-part-1" v-if="masterAccount">
                        <p>732</p> <!-- <p>{{ totalTrades }}</p> -->
                        <p>941.83%</p>
                        <p>425.00</p>
                        <p>{{ masterAccount.pips }}</p>
                        <p>$ 263.09</p>
                        <p>-$ 292.29</p>
                        <p>2h 17m</p>
                        <p>1 day ago</p>
                        <!-- <p>{{ latestTradeDuration }}</p> -->
                    </div>
                </div>
                <div class="information-content-2">
                    <div class="information-part-2">
                        <p>Longs Won</p>
                        <p>Shorts Won</p>
                        <p>Best Trade</p>
                        <p>Worst Trade</p>
                        <p>Best Trade (Pips)</p>
                        <p>Worst Trade (Pips)</p>
                    </div>
                    <div class="information-data-part-2">
                        <p>81.00%</p>
                        <p>83.00%</p>
                        <p>$ 4,071.00</p>
                        <p>-$ 1,901.00</p>
                        <p>33,255.00</p>
                        <p>-189,445.80</p>
                    </div>
                </div>
                <div class="information-content-3">
                    <div class="information-part-3">
                        <p>Profit Factor</p>
                        <p>Standard Deviation</p>
                        <p>Sharpe Ratio</p>
                    </div>
                    <div class="information-data-part-3" v-if="masterAccount">
                        <!-- <p>4.50</p> -->
                        <p>{{ masterAccount.profitFactor }}</p>
                        <p>$ 523.98</p>
                        <p>0.33</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
