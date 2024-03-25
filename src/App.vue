<script setup>
import Topbar from './components/Topbar.vue';
import AssetAllocation from './components/AssetAllocation.vue';
import FinanceSummary from './components/FinanceSummary.vue';
import ForexMarket from './components/ForexMarket.vue';
import GeneralInformation from './components/GeneralInformation.vue';
import TraderDashboard from './components/TraderDashboard.vue';
import DailyPeriods from './components/DailyPeriods.vue';
import Chart from './components/Chart.vue';
import TradesHistory from './components/TradesHistory.vue';
import TradesStatistics from './components/TradesStatistics.vue';
import './css/main.css'
import './css/responsive.css'
import { ref, onMounted } from 'vue';
import axios from 'axios';

// const liveMasters = ref({});
const liveMasters = ref([]);
const masterAccount = ref();

const fetchData = async () => {
    try {
        const response = await axios.get(`https://www.myfxbook.com/api/get-my-accounts.json?session=4gcHQQj80BSwyYjywWCy3636342`);
        liveMasters.value = response.data.accounts;
        masterAccount.value = liveMasters.value[0]
        // console.log('Live Masters:', liveMasters.value);
    } catch (error) {
        console.error('Error fetching live data:', error);
    }
}

onMounted(() => {
    fetchData(); // Fetch data when the component is mounted
    // Fetch data every second
    // setInterval(fetchData, 1000);
});

</script>

<template>
    <div class="box-wrapper" id="content">
        <Topbar/>
        <div id="box2" class="content">
            <div id="box3" class="content-left">
                <TraderDashboard
                    :liveMasters="liveMasters"
                    :masterAccount="masterAccount"
                    @update:masterAccount="masterAccount = $event"
                />
                <div id="box5" class="content-left-right">
                    <GeneralInformation
                        :masterAccount="masterAccount"
                    />
                    <TradesStatistics 
                    :masterAccount="masterAccount" 
                    />
                    <!-- <TradesStatistics 
                    :masterAccount="masterAccount" 
                    :accountId="accountId" 
                    /> -->
                </div>
            </div>
            <div id="box6" class="content-right">
                <div id="box13">
                    <div id="box7">
                        <div id="box9-1">
                            <Chart 
                                :masterAccount="masterAccount"
                            />
                            <div id="box9-2">
                                <FinanceSummary
                                    :masterAccount="masterAccount"
                                />
                                <AssetAllocation/>
                            </div>
                        </div>
                        <div id="box10">
                            <ForexMarket/>
                            <DailyPeriods/>
                        </div>
                    </div>
                    <TradesHistory
                        :masterAccount="masterAccount"
                     />
                </div>
            </div>
        </div>
    </div>
</template>
