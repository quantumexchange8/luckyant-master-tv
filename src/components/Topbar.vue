<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { onMounted } from 'vue';

const date = ref('');
const time = ref('');
const online = ref(false); // Initialize online status as false initially
const statusColor = ref('online'); // Initialize statusColor as 'online'


const updateDateTime = () => {
  var currentDate = new Date();
  var day = currentDate.getDate();
  var month = currentDate.getMonth() + 1;
  var year = currentDate.getFullYear();
  var hours = currentDate.getHours();
  var minutes = currentDate.getMinutes();
  var seconds = currentDate.getSeconds();

  day = day < 10 ? '0' + day : day;
  month = month < 10 ? '0' + month : month;
  hours = hours < 10 ? '0' + hours : hours;
  minutes = minutes < 10 ? '0' + minutes : minutes;
  seconds = seconds < 10 ? '0' + seconds : seconds;

  var dateString = day + ' . ' + month + ' . ' + year;
  var timeString = hours + ':' + minutes + ':' + seconds;

  date.value = dateString;
  time.value = timeString;
}



// async function fetchData() {
//     try {
//         const response = await axios.get('https://testmember.luckyantfxasia.com/api/getMaster');
//         const masterData = response.data.status; // Assuming status is directly available in the response data
//         console.log('Response Data Status:', masterData); // Log the status from the response data

//         // Check the status from the master data and update the online status
//         online.value = masterData === 'online';

//         // Set the statusColor based on the response data status
//         statusColor.value = masterData === 'success' ? 'online' : 'offline';
//     } catch (error) {
//         console.error('Error fetching live data:', error);
//         // Set online status to false in case of an error
//         online.value = false;
//         // Set the statusColor to red in case of an error
//         statusColor.value = 'offline';
//     }
// }



async function fetchData() {
    try {
        const response = await axios.get('https://testmember.luckyantfxasia.com/api/getMaster');
        const masterData = response.data.status; // Assuming status is directly available in the response data
        console.log('Response Data Status:', masterData); // Log the status from the response data

        // Check the status from the master data and update the online status
        online.value = masterData === 'success';

        // Set the statusColor based on the response data status
        statusColor.value = masterData === 'failed' ? 'offline' : 'online';
    } catch (error) {
        console.error('Error fetching live data:', error);
        // Set online status to false in case of an error
        online.value = false;
        // Set the statusColor to red in case of an error
        statusColor.value = 'offline';
    }
}

onMounted(() => {
    updateDateTime(); // Initial update of date and time
    fetchData(); // Fetch data when the component is mounted
    setInterval(updateDateTime, 1000); // Update date and time every second
});



</script>

<template>
    <div id="header" class="header-flex">
    <div class="header-left">
        <div class="date-n-time">
            <div id="currentDate" class="currentDate">{{ date }}</div>
            <div id="currentTime" class="currentTime">{{ time }}</div>
        </div>
    </div>
    <div class="line-left">
        <div class="line-left-white-border"></div>
    </div>
    <div class="header-center">
        <div class="icon">
            <img src="/src/assets/dashboard/icon-trans.png" alt="">
        </div>
        <div class="header-text">
            <h1>Lucky ant trading master data platform</h1>
        </div>
    </div>
    <div class="line-up-down"></div>
    <div class="line-center"></div>
    <div class="line-down-up"></div>
    <div class="header-right">
        <div :class="['status-circle', statusColor]"></div>
        <!-- <div class="label-shape"></div>
        <div id="svg-container">
            <img id="svg-img" 
             :src="svgSrc"
             alt=""
             @mouseover="handleMouseOver"
             @mouseout="handleMouseOut">
        </div> -->
        <!-- <script>
            const svgImg = document.getElementById('svg-img');
        
            // Add event listeners for mouseover and mouseout
            svgImg.addEventListener('mouseover', function() {
                // Change src to an SVG with hover effect
                svgImg.src = 'assets/svg/all-time-hover.svg'; // Change to your hover SVG file path
            });

            svgImg.addEventListener('mouseout', function() {
                // Restore original src
                svgImg.src = 'assets/svg/all-time.svg'; // Change to your original SVG file path
            });
        </script> -->
    </div>
    <div class="line-right"></div>
    <div class="parallelogram-shape">
        <div class="parallelogram-left-dark"></div>
        <div class="parallelogram-left-light"></div>
        <div class="parallelogram-right-light"></div>
        <div class="parallelogram-right-dark"></div>
    </div>
</div>
</template>


