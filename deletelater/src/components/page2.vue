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
                labels: ['total # genes', 'total known functions'],
                datasets: [{
                    label: '# of Gene Families within range of Genes',
                    data: [],
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255,99,132,1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
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
        created() {
        // this.loadQuote();
    },
    methods: {
        loadQuote() {
            this.status = 'Loading...';
            axios.get('http://54.68.67.235:3000/api/panther/annotations')
            .then( (response) => {
                console.log(response);
                this.status = 'Done Loading.';
                let docs = response.data.response.docs;
                let knownFunctionCount = 0;
                let elementCount = 0;
                docs.forEach(element => 
                {
                    // console.log(element.id);
                    if (element.species_list != null)
                        knownFunctionCount += 1;
                    elementCount += 1;
                });
                console.log(elementCount);
                console.log(knownFunctionCount);
                this.charData.data.datasets[0].data.push(elementCount);
                this.charData.data.datasets[0].data.push(knownFunctionCount);
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
    /* #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
    #content {
        margin: auto;
        width: 1024px;
        background-color: #FFFFFF;
        padding: 20px;
    } */
</style>