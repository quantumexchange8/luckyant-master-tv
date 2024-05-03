<script setup>

const props = defineProps({
    masterAccount: Object
})

const formatTradeDate = (dateString) => {
    const tradeTimestamp = Date.parse(dateString);
    const todayTimestamp = Date.now();
    const millisecondsPerDay = 1000 * 60 * 60 * 24;
    const durationDays = Math.floor((todayTimestamp - tradeTimestamp) / millisecondsPerDay);

    return durationDays;
};

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
                        <p>Average Profit</p>
                        <p>Average Loss</p>
                        <p>Last 30 Days Total Trade</p>
                        <p>Latest Trade</p>
                    </div>
                    <div class="information-data-part-1" v-if="masterAccount">
                        <p>{{ masterAccount.totalTrade }}</p>
                        <p>{{ masterAccount.profitability }}</p>
                        <p>{{ masterAccount.totalLot }}</p>
                        <p>$ {{ masterAccount.averageProfit }}</p>
                        <p>$ {{ masterAccount.averageLoss }}</p>
                        <p>{{ masterAccount.totalTradesLast30Days }}</p>
                        <p>{{ formatTradeDate(masterAccount.latestTrade) }} days ago</p>
                    </div>
                </div>
                <div class="information-content-2">
                    <div class="information-part-2">
                        <p>Longs Won</p>
                        <p>Shorts Won</p>
                        <p>Best Trade</p>
                        <p>Worst Trade</p>
                        <p>Total Longs Trade</p>
                        <p>Total Shorts Trade</p>
                    </div>
                    <div class="information-data-part-2" v-if="masterAccount">
                        <p>{{ '(' + masterAccount.longsWon + '/' +masterAccount.totalLongsTrade + ')' + ' ' + masterAccount.longsWonPercentage }} %</p>
                        <p>{{ '(' + masterAccount.shortsWon + '/' +masterAccount.totalShortsTrade + ')' + ' ' + masterAccount.shortsWonPercentage }} %</p>
                        <p>$ {{ masterAccount.bestTrade }}</p>
                        <p>$ {{ masterAccount.worstTrade }}</p>
                        <p>{{ masterAccount.totalLongsTrade }}</p>
                        <p>{{ masterAccount.totalShortsTrade }}</p>
                    </div>
                </div>
                <div class="information-content-3">
                    <div class="information-part-3">
                        <p>Profit Factor</p>
                        <p>Standard Deviation</p>
                        <!-- <p>Sharpe Ratio</p> -->
                    </div>
                    <div class="information-data-part-3" v-if="masterAccount">
                        <p>{{ masterAccount.profitFactor }}</p>
                        <p>$ {{ masterAccount.standardDeviation }}</p>
                        <!-- <p>0.33</p> -->
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
