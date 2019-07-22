<template>
  <div class="chart2">
    <div id="chart2"></div>
    <p>
      各门店租车量走势
      <span class="chart_can">二季度</span>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dataX: [
        // "4月", "5月", "6月"
        ],
      dataY: [
        // {
        //   data: [30, 50, 70],
        //   type: "line",
        //   smooth: false,
        //   color: "green",
        //   name: "金水区店"
        // },
        // {
        //   data: [70, 50, 50],
        //   type: "line",
        //   smooth: false,
        //   color: "red",
        //   name: "惠济区店"
        // },
        // {
        //   data: [50, 65, 80],
        //   type: "line",
        //   smooth: false,
        //   color: "skyblue",
        //   name: "二七区店"
        // }
      ]
    };
  },
  mounted() {
    this.$axios.get('https://www.easy-mock.com/mock/5d316fc415f582091bfe32f8/car_pc/carNum').then(
      (res)=>{
        this.dataX = res.data.numX;
        this.dataY = res.data.numY;
        this.$chart.line2("chart2", this.dataX, this.dataY);
        // console.log(res);
      }).catch((err)=>{
        throw err
      })
  },
  methods: {},
  components: {}
};
</script>

<style scoped lang="less">
.chart2 {
  position: relative;
  width: 65%;
  #chart2 {
    min-width: 548px;
    // width: 548px;
    height: 370px;
    background: white;
  }
  p {
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    font-size: 18px;
    overflow: hidden;
    padding: 0 31px 0 18px;
    margin-top: 19px;
    box-sizing: border-box;
    .chart_can {
      font-size: 12px;
      float: right;
      color: grey;
      padding-right:10px;
      background: url('../../assets/hometex/jiantou2.png') no-repeat right center; 
    }
  }
}
</style>
hei