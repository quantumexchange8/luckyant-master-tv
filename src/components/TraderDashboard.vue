<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios'; // Import Axios

const props = defineProps({
    liveMasters: Object,
    masterAccount: Object,
})

const master = ref();
const emit = defineEmits(['update:masterAccount'])
const getMaster = (account) => {
    master.value = account
    emit('update:masterAccount', master.value)
}

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

// onMounted(() => {
//     applyColorToTotalGrowth();
// });


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
        <div
            v-for="(user, index) in liveMasters"
            :key="index"
            class="trader-dashboard-frame"
            :class="{ 'clicked': user === master || user === masterAccount}"
            @click="getMaster(user)"

        >
            <div
                class="pamm-master-box"
            >
                <div class="dashboard-name-border">
                    <div class="dashboard-name">
                        <h4>{{ user.name }}</h4>
                    </div>
                </div>
                <div class="trader-dashboard-content">
                    <div class="trader-dashboard-img-1">
                        <div class="shadow-container">
                            <div class="hexagon-border">
                                <img :src="`/assets/ProfileImage${index + 1}.png`" alt="">
                            </div>
                        </div>
                    </div>
                    <div class="content-up-down">
                        <div class="add-onbox">
                            <div class="trader-dashboard-content-right">
                                <div class="trader-dashboard-content-right-name">
                                    <p>Balance</p>
                                   <!-- <p>Investors</p> -->
                                </div>
                            </div>
                            <div class="trader-dashboard-content-right-data">
                                <p>$ {{ user.balance }} </p>
                               <!-- <p>{{ user.subscriber }}</p> -->
                            </div>
                        </div>
                        <div class="bar-chart-data">
                            <div class="bar-chart">
                                <div class="bar-chart-container">
                                    <img id="svg-img" src="/src/assets/svg/barchart.svg" alt="">
                                </div>
                            </div>
                            <div class="bar-chart-profitability">
                                <p>Profitability</p>
                                <p class="font-green-color">{{ user.profitability }}%</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- <div class="trader-dashboard-frame">
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
            <div class="trader-dashboard-frame">
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
        </div> -->
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
