<script setup>
import axios from "axios";
import {onMounted, ref} from "vue";

const metaInformation = ref();
const session = ref('');
const email = ref('official@luckyantfxasia.com');
const password = ref('72l3SGK=f;8V'); // Your original password

// const authSession = async () => {
//     try {
//         const response = await axios.get(`https://www.myfxbook.com/api/login.json?email=${email.value}&password=${password.value}`);
//         session.value = response.data;
//         console.log(response.data)
//     } catch (error) {
//         console.error('Error fetching live data:', error);
//     }
// }
//
// authSession();

const fetchData = async () => {
    try {
        console.log(session.value)
        const response = await axios.get(`https://www.myfxbook.com/api/get-my-accounts.json?session=4gcHQQj80BSwyYjywWCy3636342`);
        metaInformation.value = response.data.accounts[0];
        console.log(metaInformation.value)
    } catch (error) {
        console.error('Error fetching live data:', error);
    }
}

onMounted(() => {
    fetchData(); // Fetch data when the component is mounted
});

const metaInformationDate = (date) => {
    const dateString = date;
    const dateObject = new Date(dateString);
    return `${dateObject.getMonth() + 1}/${dateObject.getDate()}/${dateObject.getFullYear()}`;
}

const daysSinceFirstTrade = (dateString) => {
    const date = new Date(dateString); // Parse dateString into a Date object
    console.log(date);
    const now = new Date(); // Assuming you want to get the current date
    const differenceInTime = now.getTime() - date.getTime();
    return Math.floor(differenceInTime / (1000 * 3600 * 24));
};

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
                <div class="information-content-left">
                    <div class="information-part-1">
                        <p>Profit</p>
                        <p>Floating P/L</p>
                        <p>Equity</p>
                        <p>Balance</p>
                        <p>Deposit</p>
                        <p>Withdrawal</p>
                    </div>
                    <div class="information-part-2">
                        <p>Total Growth</p>
                        <p>Current Month Growth</p>
                        <p>Max. Daily Growth</p>
                        <p>Current Drawdown</p>
                        <p>Max. Drawdown</p>
                    </div>
                    <div class="information-part-3">
<!--                        <p>Investors</p>-->
                        <p>Leverage</p>
                        <p>Performance Fee</p>
                        <p>Started Date</p>
                        <p>Active Days</p>
<!--                        <p>Latest Activity</p>-->
                    </div>
                </div>
                <div v-if="metaInformation" class="information-content-right">
                    <div class="information-data-part-1">
                        <p>$ {{ metaInformation.profit }}</p>
                        <p>$ {{ metaInformation.equity - metaInformation.balance }}</p>
                        <p>$ {{ metaInformation.equity }}</p>
                        <p>$ {{ metaInformation.balance }}</p>
                        <p>$ {{ metaInformation.deposits }}</p>
                        <p>$ {{ metaInformation.withdrawals }}</p>
                    </div>
                    <div class="information-data-part-2">
                        <p class="font-green-color">+941.83%</p>
                        <p class="font-green-color">+10.36%</p>
                        <p class="font-green-color">+0.75%</p>
                        <p>0.07%</p>
                        <p>7.13%</p>
                    </div>
                    <div class="information-data-part-3">
<!--                        <p>100</p>-->
                        <p>1:500</p>
                        <p>40%</p>
                        <p>{{ metaInformationDate(metaInformation.firstTradeDate) }}</p>
                        <p>{{ daysSinceFirstTrade (metaInformation.firstTradeDate) }} Days</p>
<!--                        <p>30 mins ago</p>-->
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>