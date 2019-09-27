<template>
  <div class="about">
    <button @click="getEchartData">点我</button>
     <div id="myChart" style="width:100%;height:800px"></div>
  </div>
</template>

<script>
// @ is an alias to /src
  import https from '@/https.js';
  import echarts from 'echarts'
  export default {
    name: 'home',
    components: {
    },
    mounted(){
      let arr=this.dir.split('\\');
      this.cname=arr[arr.length-1];
    },
    data(){
      return {
        dir:'C:\\Users\\Administrator\\Desktop\\shy_pm\\dist',
        cname:'',
      }
    },
    methods:{
      getEchartData(){
        let params={
          dir:this.dir
        }
        https.fetchPost('/getConstruction',params).then((res)=>{
          console.log(res);
          let obj={
            name:this.cname,
            children:res.data
          }
          const myChart = echarts.init(document.getElementById('myChart'));
          const option = {
            series : [
              {
                type: 'tree',

                data: [obj],

                top: '1%',
                left: '7%',
                bottom: '1%',
                right: '20%',

                symbolSize: 7,

                label: {
                  normal: {
                    position: 'left',
                    verticalAlign: 'middle',
                    align: 'right',
                    fontSize: 12
                  }
                },

                leaves: {
                  label: {
                    normal: {
                      position: 'right',
                      verticalAlign: 'middle',
                      align: 'left'
                    }
                  }
                },

                expandAndCollapse: true,
                animationDuration: 550,
                animationDurationUpdate: 750
              }
            ]
          }
          myChart.setOption(option);
          window.addEventListener('resize', function () {
            myChart.resize()
          })
        })
      },
    
    }
  }
</script>
