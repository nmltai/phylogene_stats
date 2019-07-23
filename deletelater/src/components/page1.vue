<template>
  <div>
    <p>{{status}}</p>
    <canvas ref="chart"></canvas>
  </div>
</template>

<script>
  import Chart from 'chart.js';
  import { Bar } from 'vue-chartjs'
  const axios = require('axios');
  export default {
    data: () => ({
      status: '',
      charData:{
        labels: ['0-24', '25-49', '50-74', '75-99', '100-124','125-149','150-174','175-199', '200-224', 
  '225-249', '250-274', '275-299','300-324', '325-349', '350-374', '375-499', '500-749', '750-999', '1000-1499', 
  '1500-1999', '2000-2499', '2500-2999', '3000+'],
        datasets: [{
          label: '# of Gene Families within range of Genes Count',
          data: [],
          backgroundColor: [
            'rgba(255, 99, 132, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(255, 206, 86, 0.2)',
            'rgba(75, 192, 192, 0.2)',
            'rgba(153, 102, 255, 0.2)',
            'rgba(255, 159, 64, 0.2)',
            'rgba(255, 99, 132, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(255, 206, 86, 0.2)',
            'rgba(75, 192, 192, 0.2)',
            'rgba(153, 102, 255, 0.2)',
            'rgba(255, 159, 64, 0.2)',
            'rgba(255, 99, 132, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(255, 206, 86, 0.2)',
            'rgba(75, 192, 192, 0.2)',
            'rgba(153, 102, 255, 0.2)',
            'rgba(255, 159, 64, 0.2)',
            'rgba(255, 99, 132, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(255, 206, 86, 0.2)',
            'rgba(75, 192, 192, 0.2)',
            'rgba(255, 99, 132, 0.2)'
          ],
            borderColor: [
              'rgba(255,99,132,1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)',
              'rgba(255, 159, 64, 1)',
              'rgba(255,99,132,1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)',
              'rgba(255, 159, 64, 1)',
              'rgba(255,99,132,1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)',
              'rgba(255, 159, 64, 1)',
              'rgba(255,99,132,1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)'
            ],
            borderWidth: 1
        }]
      },
    }),
      extends: Bar,
      mounted() {
        var chart = this.$refs.chart;
        var ctx = chart.getContext("2d");
        var myChart = new Chart(ctx, {
          type: 'bar',
          data: this.charData,
          options: {
            scales: {
              yAxes: [{
                ticks: {
                  beginAtZero: true
                }
              }]
            }
          }
        });
        // this.loadQuote();
      },
      created() {
      // this.loadQuote();
      },
    methods: {
      loadQuote() {
        this.status = 'Loading...';
        axios.get('http://54.68.67.235:3000/api/panther/ids')
        .then( (response) => {
          console.log(response);
          this.status = 'Done Loading.';
          let docs = response.data.response.docs;
          // docs = docs.slice(0, 10);  
          let ARRAY_LENGTH = 23;
          var count = new Array(ARRAY_LENGTH);
          for (var i = 0; i < count.length; i++) {
            count[i] = 0;
          }
          docs.forEach(element => 
          {
            let start_range = 0;
            let end_range = 24; 
            for (var i = 0; i < 16; i++) 
            {
              if (element.uniprot_ids.length <= end_range && element.uniprot_ids.length >= start_range)
                count[i] += 1;
              end_range += 25;
              start_range += 25;
            }
            if (element.uniprot_ids.length <= 749 && element.uniprot_ids.length >= 500)
              count[16] += 1;
            if (element.uniprot_ids.length <= 999 && element.uniprot_ids.length >= 750) 
              count[17] += 1;
            if (element.uniprot_ids.length <= 1499 && element.uniprot_ids.length >= 1000) 
              count[18] += 1;
            if(element.uniprot_ids.length <= 1999 && element.uniprot_ids.length >= 1500) 
              count[19] += 1;
            if(element.uniprot_ids.length <= 2499 && element.uniprot_ids.length >= 2000) 
              count[20] += 1;
            if(element.uniprot_ids.length <= 2999 && element.uniprot_ids.length >= 2500) 
              count[21] += 1;
            if(element.uniprot_ids.length <= 500000 && element.uniprot_ids.length >= 3000) 
              count[22] += 1;
          });
          for (var i = 0; i < ARRAY_LENGTH; i++) {
            // console.log(count[i]);
            this.charData.datasets[0].data.push(count[i]);
          }
          var chart = this.$refs.chart;
          var ctx = chart.getContext("2d");
          var myChart = new Chart(ctx, {
            type: 'bar',
            data: this.charData,
            options: {
              scales: {
                yAxes: [{
                  ticks: {
                    beginAtZero: true
                  }
                }]
              }
            }
          });
        })
      .catch(function (error) {
        console.log('An error has ocurred.(page1) ' + error);
      });
      }
    }
  }
</script>

<style>
</style>