<script setup>
import { onMounted } from 'vue';

function applyColorToForexMarket() {
    const percentages = document.querySelectorAll('.content-forex-market-left .percentage p'); // Select all percentage elements
    const values = document.querySelectorAll('.content-forex-market-left .value p'); // Select all value elements

    for (let i = 0; i < percentages.length; i++) {
        const percentage = parseFloat(percentages[i].textContent); // Get the percentage value as float
        const value = values[i].textContent.trim(); // Get the value and trim any leading/trailing spaces
        const lastDigit = value[value.length - 1]; // Get the last digit of the value

        // Apply color to the percentage
        if (percentage > 0) {
            percentages[i].classList.add("forex-market-green-percentage");
        } else if (percentage < 0) {
            percentages[i].classList.add("font-red-color-percentage");
        }

        // Apply color to the last digit of the value
        if (percentage > 0) {
            values[i].innerHTML = value.substring(0, value.length - 1) + `<span class="forex-market-green">${lastDigit}</span>`;
        } else if (percentage < 0) {
            values[i].innerHTML = value.substring(0, value.length - 1) + `<span class="font-red-color">${lastDigit}</span>`;
        }
    }
}

onMounted(() => {
    applyColorToForexMarket();
});

</script>

<template>
    <div id="box11" class="content-forex-market">
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
                <h3>Major Forex Market</h3>
            </div>
        </div>
       
        <iframe src="https://ssltsw.investing.com?lang=1&forex=1,2,3,5,7,9,10&commodities=8830,8836,8831,8849,8833,8862,8832&indices=175,166,172,27,179,170,174&stocks=345,346,347,348,652,355,22402&tabs=2" width="100%" height="280" style="background: transparent;" allowtransparency="true" scrolling="no"></iframe>


    </div>
</template>