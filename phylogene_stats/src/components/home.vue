<template>
  <div>
    <canvas ref="chart"></canvas>
    <!-- <p>{{status}}</p> -->
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
        labels: ['1-10', '11-100', '101-500', '501-1000', '1001-2000', '2000+'],
        datasets: [{
          label: '# of Gene Families within Range of Gene Count',
          data: [],
          backgroundColor: [
            'rgba(54, 162, 235, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(54, 162, 235, 0.2)',
          ],
            borderColor: [
              'rgba(54, 162, 235, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(54, 162, 235, 1)',
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

          let ARRAY_LENGTH = 6;
          var count = new Array(ARRAY_LENGTH);
          for (var i = 0; i < count.length; i++) {
            count[i] = 0;
          }
          docs.forEach(element => 
          {
            if (element.uniprot_ids.length <= 10 && element.uniprot_ids.length >= 1)
              count[0] += 1;
            if (element.uniprot_ids.length <= 100 && element.uniprot_ids.length >= 11) 
              count[1] += 1;
            if (element.uniprot_ids.length <= 500 && element.uniprot_ids.length >= 101) 
              count[2] += 1;
            if(element.uniprot_ids.length <= 1000 && element.uniprot_ids.length >= 501) 
              count[3] += 1;
            if(element.uniprot_ids.length <= 2000 && element.uniprot_ids.length >= 1001) 
              count[4] += 1;
            if(element.uniprot_ids.length <= 50000000 && element.uniprot_ids.length >= 2000) 
              count[5] += 1;
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