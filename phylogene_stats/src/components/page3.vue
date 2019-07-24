<template>
    <div>
      <chart :options="chartOptionsBar"></chart>
      <p>{{status}}</p>
    </div>
</template>

<script>
const axios = require('axios');
export default {
  name: 'home',
  data: () => ({
  status: '',
  chartOptionsBar: {
      tooltip : {
      trigger: 'axis',
      axisPointer : {           
          type : 'shadow'        
      }
    },
    xAxis: {
      data: [],
      axisTick: {
          alignWithLabel: true
      },
      name: '# of genes in a range',
      nameLocation: 'middle',
      nameGap: '20'
    },
    yAxis: {
      type: 'value',
      name: '# of families'
    },
    series: [
      {
        name: 'gene range',
        type: 'bar',
        stack: '总量',
        label: {
            normal: {
              show: true,
              position: 'inside'
            }
        },
        data: []
      }       
    ],
        grid: {
        left: '10%',
        right: '1%',
        bottom: '1%',
        containLabel: true
    },
    title: {
      text: 'How many families fit under a certain # of genes?',
      x: 'center',
      textStyle: {
        fontSize: 18
      }
    },
    color: ['#800000']
  } 
}),
  created() {
    // this.loadQuote();
  },
  methods: {
    loadQuote() {
      this.status = 'Loading...';
      axios.get('http://54.68.67.235:3000/api/panther/ids')
      .then( (response) => {
        this.status = 'Done Loading.';
        let docs = response.data.response.docs;
        // docs = docs.slice(0, 10);

        let ARRAY_LENGTH = 23;
        var count = new Array(ARRAY_LENGTH);
        for (var i = 0; i < count.length; i++) {
          count[i] = 0;
        }
        docs.forEach(element => {
          // console.log(element.uniprot_ids.length);

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
            for (var i = 0; i < ARRAY_LENGTH; i++) 
            {
              // console.log(count[i]);
              this.chartOptionsBar.series[0].data.push(count[i]);
            }
      })
      .catch(function (error) {
        console.log('An error has ocurred.(home) ' + error);
      });
    }
  }
}
</script>

<style>
</style>
