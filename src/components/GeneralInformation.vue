<script setup>

// import { ref, onMounted, watch } from 'vue';

const props = defineProps({
    masterAccount: Object
})

const metaInformationDate = (date) => {
    const dateString = date;
    const dateObject = new Date(dateString);
    return `${dateObject.getMonth() + 1}/${dateObject.getDate()}/${dateObject.getFullYear()}`;
}

const daysSinceFirstTrade = (dateString) => {
    const date = new Date(dateString); // Parse dateString into a Date object
    // console.log(date);
    const now = new Date(); // Assuming you want to get the current date
    const differenceInTime = now.getTime() - date.getTime();
    return Math.floor(differenceInTime / (1000 * 3600 * 24));
};

// watch(() => props.masterAccount, (newMasterAccount) => {
//     if (newMasterAccount) {
//         console.log('Master Account ID:', newMasterAccount.id);
//     }
// });

</script>

<template>
    <div class="content-right-up">
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
                <h3>GENERAL INFORMATION</h3>
            </div>
        </div>
        <div class="trader-dashboard-frame">
            <div class="information-content">
                <div class="information-content-1">
                    <div class="information-part-1">
                        <p>Profit</p>
                        <!-- <p>Floating P/L</p> -->
                        <p>Equity</p>
                        <p>Balance</p>
                        <p>Deposit</p>
                        <p>Withdrawal</p>
                    </div>
                    <div class="information-data-part-1"  v-if="masterAccount">
                        <p>$ {{ masterAccount.totalProfit }}</p>
                        <!-- <p>$ {{ masterAccount.floating }}</p> -->
                        <p>$ {{ masterAccount.equity }}</p>
                        <p>$ {{ masterAccount.balance }}</p>
                        <p>$ {{ masterAccount.deposits }}</p>
                        <p>$ {{ masterAccount.withdrawals }}</p>
                    </div>
                </div>
                <div class="information-content-2">
                    <div class="information-part-2">
                        <p>Total Growth</p>
                        <p>Current Month Growth</p>
                        <p>Max. Daily Growth</p>
                        <!-- <p>Current Drawdown</p>
                        <p>Max. Drawdown</p> -->
                    </div>
                    <div class="information-data-part-2" v-if="masterAccount">
                        <p :class="{
                            'font-green-color': masterAccount.totalGrowth > 0,
                            'font-red-color': masterAccount.totalGrowth < 0
                        }">
                            {{ masterAccount.totalGrowth !== 0 ? (masterAccount.totalGrowth > 0 ? '+' + masterAccount.totalGrowth : masterAccount.totalGrowth) : masterAccount.totalGrowth }}%
                        </p>
                        <p :class="{
                            'font-green-color': masterAccount.currentMonthGrowth > 0,
                            'font-red-color': masterAccount.currentMonthGrowth < 0
                        }">
                            {{ masterAccount.currentMonthGrowth !== 0 ? (masterAccount.currentMonthGrowth > 0 ? '+' + masterAccount.currentMonthGrowth : masterAccount.currentMonthGrowth) : masterAccount.currentMonthGrowth }}%
                        </p>
                        <p :class="{
                            'font-green-color': masterAccount.maxDailyGrowth > 0,
                            'font-red-color': masterAccount.maxDailyGrowth < 0
                        }">
                            {{ masterAccount.maxDailyGrowth !== 0 ? (masterAccount.maxDailyGrowth > 0 ? '+' + masterAccount.maxDailyGrowth : masterAccount.maxDailyGrowth) : masterAccount.maxDailyGrowth }}%
                        </p>
                        <!-- <p>0.07%</p>
                        <p>7.13%</p> -->
                    </div>
                </div>
                <div class="information-content-3">
                    <div class="information-part-3">
                        <!--<p>Investors</p>-->
                        <p>Leverage</p>
                        <!-- <p>Performance Fee</p> -->
                        <p>Started Date</p>
                        <p>Active Days</p>
                        <!--<p>Latest Activity</p>-->
                    </div>
                    <div class="information-data-part-3"  v-if="masterAccount">
                        <p>1:{{masterAccount.marginLeverage}}</p>
                        <!-- <p>40%</p> -->
                        <p>{{ metaInformationDate(masterAccount.startDate) }}</p>
                        <p>{{ daysSinceFirstTrade (masterAccount.startDate) }} Days</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>