<template>
  <div class="chart1">
    <div id="chart1"></div>
    <p>
      今日车辆数据
      <span class="right">查看更多</span>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      num:'',
      topData: [
        // { value: 10, name: "代取车辆：" + 10 },
        // { value: 23, name: "在租车辆：" + 23 },
        // { value: 9, name: "逾期未还：" + 9 },
        // { value: 58, name: "未租车辆：" + 58 }
      ]
    };
  },
  mounted() {
    this.$axios.get(
      'https://www.easy-mock.com/mock/5d316fc415f582091bfe32f8/car_pc/carNum'
    ).then((res)=>{
      this.topData = res.data.num
      this.$chart.line1("chart1", this.topData);
      // console.log(res.data.num);
      // console.log(this);
    }).catch((err)=>{
      console.log(err)
    });

  },
  methods: {},
  components: {}
};
</script>

<style scoped lang="less">
.chart1 {
  width: 40%;
  position: relative;
  p {
    position: absolute;
    top: 10px;
    left: 20px;
    width: 100%;
    display: block;
    box-sizing: border-box;
    padding: 0 40px 0 10px;
    overflow: hidden;
    font-size: 19px;
    line-height: 19px;
    .right {
      font-size: 13px;
      float: right;
    }
  }
  #chart1 {
    min-width: 342px;
    height: 100%;
    min-height: 191px;
    background: white;
  }
}
</style>
