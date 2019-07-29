<template>
    <div>
        <p>{{status}}</p>
        <canvas ref="chart"></canvas>
    </div>
</template>

<script>
    import Chart from 'chart.js';
    import { Bar } from 'vue-chartjs';
    import { constants } from 'crypto';
    const axios = require('axios');
    export default {
        data: () => ({
        status: '',
        charData:{
            data: {
                labels: ['Total # of Gene Families', 'Total # of Families with at least 1 known function (plant & nonplant)'],
                datasets: [{
                    label: '# of Gene Families',
                    data: [],
                    backgroundColor: [
                        'rgba(255, 159, 64, 0.2)',
                        'rgba(255, 159, 64, 0.2)',
                    ],
                    borderColor: [
                        'rgba(255, 159, 64, 1)',
                        'rgba(255, 159, 64, 1)',
                    ],
                    borderWidth: 1
                }]
            },
        }
        }),
        extends: Bar,
        mounted() {
            var chart = this.$refs.chart;
            var ctx = chart.getContext("2d");
            var myChart = new Chart(ctx, {
                type: 'bar',
                data: this.charData.data,
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
            this.loadQuote();
        },
        // created() {
        // this.loadQuote();
    // },
    methods: {
        loadQuote() {
            this.status = 'Loading...';
            axios.get('http://35.165.70.47:8080/panther/annotation')
            .then( (response) => {
                // console.log(response);
                this.status = 'Done Loading.';
                let docs = response.data;
                let knownFunctionCount = 0;
                let elementCount = 0;
                docs.forEach(element => 
                {
                    // console.log(element.id);
                    if (element.go_annotations_count != null)
                        knownFunctionCount += 1;
                    elementCount += 1;
                });
                // console.log(elementCount);
                // console.log(knownFunctionCount);
                this.charData.data.datasets[0].data.push(elementCount);
                this.charData.data.datasets[0].data.push(knownFunctionCount);

                var chart = this.$refs.chart;
                var ctx = chart.getContext("2d");
                var myChart = new Chart(ctx, {
                  type: 'bar',
                  data: this.charData.data,
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
            .catch(function (error) 
            {
                console.log('An error has ocurred.(page2) ' + error);
            });
        }
    }
    }
</script>

<style>
</style>