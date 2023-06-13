<template>
  <div>
    <div id="ecd"></div>
    <div class="tips">
        <img  src="../assets/images/weizhi@3x.png" v-if="proValue==0" alt="">
        <img  src="../assets/images/zhengchang@3x.png" v-if="proValue>0 && proValue<=50" alt="">
        <img  src="../assets/images/baojing@3x.png" v-if="proValue>50" alt="">
        <div class="status">
            <p>检测状态</p>
            <p>{{ status }}</p>
        </div>
    </div>
    <input type="range" min="0" max="100" v-model="proValue" @input="changeValue">
  </div>
</template>

<script setup lang="ts">
import { onMounted,watch,ref } from 'vue'
import * as echarts from 'echarts';
let myChart
const echartData =[
{
    value: 0,
    name: '异常分数',
    title: {
      offsetCenter: ['0%', '10%']
    },
    detail: {
      valueAnimation: true,
      offsetCenter: ['0%', '-15%']
    }
  }
]
let status =ref('未知')
let proValue =ref(0)
let option = {
  series: [
    {
      type: 'gauge',
      startAngle: 90,
      endAngle: -270,
      pointer: {
        show: false
      },
      progress: {
        show: true,
        overlap: false,
        roundCap: true,
        clip: true,
        itemStyle: {
          color:{
            type: 'linear',
            x: 0,
            y: 0,
            x2: 0,
            y2: 1,
            colorStops: [{
                offset: 0, color: 'rgba(154,112,243)' 
            }, {
                offset: 1, color: 'rgba(165,132,247,.7)' 
            }],
            global: false 
            }
        }
      },
      axisLine: {
        lineStyle: {
          width: 30,
          shadowColor:'rgba(212,212,232)',
          shadowOffsetY:5
        }
      },
      splitLine: {
        show: false,
        distance: 0,
        length: 10
      },
      axisTick: {
        show: false
      },
      axisLabel: {
        show: false,
        distance: 50
      },
      data: echartData,
      title: {
        fontSize: 14,
        offsetCenter:[0,0]
      },
      detail: {
        fontSize: 28,
        color: 'inherit',
        borderColor: 'inherit',
        borderRadius: 20,
        borderWidth: 0,
        formatter: '{value}%'
      }
    }
  ]
};
let changeValue =(e) =>{
    console.log('e: ', e);
    echartData[0].value=proValue.value;
    myChart.setOption(option)
}
onMounted(() => {
    myChart = echarts.init(document.getElementById('ecd'));
    echartData[0].value=proValue.value;
    myChart.setOption(option)
})
</script>
<style lang="less">
#ecd{
    width: 300px;
    height: 300px;
}
.tips{
    display: flex;
    justify-content: center;
    img{
        width: 50px;
        height: 50px;
    }
}
</style>