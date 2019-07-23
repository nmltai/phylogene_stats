<template>
    <div>
        <h1>page2</h1>
        <div class="chart-wrapper">
            <chart :options="chartOptionsPie"></chart>
        </div>
    </div>
</template>

<script>
export default {
  name: 'page2',
   data: () => ({
    chartOptionsPie: {
        title : {
        text: 'Pokemon',
        subtext: 'Hoenn',
        x:'center'
    },
    tooltip : {
        trigger: 'item',
        formatter: "{a} <br/>{b} : {c} ({d}%)"
    },
    legend: {
        orient: 'vertical',
        left: 'left',
        data: ['fire','ghost','water','fighting','grass']
    },
    series : [
        {
            name: '访问来源',
            type: 'pie',
            radius : '55%',
            center: ['50%', '60%'],
            data:[
                {value:335, name:'fire'},
                {value:310, name:'ghost'},
                {value:234, name:'water'},
                {value:135, name:'fighting'},
                {value:1548, name:'grass'}
            ],
            itemStyle: {
                emphasis: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
}
})
}
</script>