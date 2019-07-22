<template>
  <div class="reg">
    <div class="reg_top">
      <input type="text" :placeholder="carNunber" v-model="carVal" />
      <button @click="changes" :class="{'clors':colorshow}">{{carTex}}</button>
    </div>
    <div class="reg_one" v-show="divshow">
      <h2>最近车辆出租记录</h2>
      <ul>
        <li class="reg_nav">
          <span>车型</span>
          <span>车牌号</span>
          <span>订单号</span>
          <span>借车时间</span>
          <span>还车时间</span>
          <span>状态</span>
          <span>是否有违章</span>
        </li>
        <li v-for="(item, index) in carall" :key="index">
          <span>{{item.vehicle}}</span>
          <span>{{item.plateNumber}}</span>
          <span>{{item.orderNumber}}</span>
          <span>{{item.carTime}}</span>
          <span>{{item.collectionTime}}</span>
          <span>{{item.huanCheZhuangTai}}</span>
          <span>{{item.failure}}</span>
        </li>
      </ul>
    </div>
    <div class="reg_two" v-show="!divshow">
      <div class="reg_two_l">
        <h2>车辆基本信息</h2>
        <img src="../../assets/mendian/che.png" alt />
        <p>品牌：{{carLeft.carBrand}}/{{carLeft.bsx}}</p>
        <p>车牌号：{{carNunber}}</p>
        <p>发动机号码：{{carLeft.engineNum}}</p>
        <p>注册号码：{{carLeft.openingTime}}</p>
        <p>发证时间：{{carLeft.registerTime | dates}}</p>
        <p>车主：{{carLeft.owner}}</p>
        <p>门店：{{carLeft.store}}</p>
      </div>
      <div class="reg_two_r">
        <h2>车辆出租记录（共 {{carRight.length}} 次）</h2>
        <ul>
          <li>
            <span class="reg_two_r_nav">借出日期</span>
            <span>归还日期</span>
            <span>订单号</span>
          </li>
          <li v-for="(item, index) in carRight" :key="index">
            <span>{{item.collectionTime | dates}}</span>
            <span>{{item.carTime | dates}}</span>
            <span>{{item.jinjishouji}}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      divshow: true,
      colorshow: false,
      // input 的plackholder
      carNunber: "请输入车牌号",
      carTex: "车辆查询",
      // input 的内容
      carVal: "",
      // 车辆信息
      carall:[],
      carLeft: {},
      carRight: [],
    };
  },
  mounted(){
    this.$axios.get('http://172.25.1.194:8080/order/zuijinchuzu')
    .then((res)=>{
      // console.log(res);
      // console.log(res.data);
      this.carall = res.data;
    }).catch((err)=>{
      throw err
    })
  },
  methods: {
    changes() {
      this.$axios
        .get(
          `http://${this.$store.state.id}/order/getinfobycarnum?carNum=${this.carVal}`
        )
        .then(res => {
          // console.log(this.carVal);
          this.carLeft = res.data[0];
          this.carRight = res.data[1];
          // console.log(this.carLeft);
          // console.log(this.carRight);
        });
      this.divshow = !this.divshow;
      this.colorshow = !this.colorshow;
      if (this.carTex == "车辆查询") {
        this.carTex = "返回查询";
        this.carNunber = this.carVal;
        this.carVal = '';
      } else if(this.carTex == '返回查询'){
        this.carNunber = '请输入车牌号'
        this.carTex = "车辆查询";
      }
    }
  },
  filters: {
    dates(val) {
      let date = new Date(val);
      let year = date.getFullYear();
      let month = date.getMonth() + 1;
      let day = date.getDate();
      month = String(month).padStart(2, "0");
      day = String(day).padStart(2, "0");
      return year + "-" + month + "-" + day;
    },
    times(val) {
      let date = new Date(val);
      let hour = date.getHours();
      let min = date.getMinutes();
      hour = String(hour).padStart(2, "0");
      min = String(min).padStart(2, "0");
      return hour + ":" + min;
    }
  },
  components: {}
};
</script >

<style scoped lang='less'>
.reg {
  padding: 35px 36px 0 42px;
  .reg_top {
    box-sizing: border-box;
    width: 100%;
    margin-bottom: 33px;
    padding: 22px 0 17px 55px;
    background: white;
    box-shadow: 0px 0px 12px 1px rgba(21, 21, 21, 0.23);
    input {
      width: 40%;
      min-width: 342px;
      height: 30px;
      padding-left: 40px;
      background: url("../../assets/search/search.png") no-repeat 5% center,
        #fbf5e0;
      margin-right: 18px;
      border-radius: 15px;
    }
    .clors {
      background: #ccc;
    }
    button {
      font-size: 13px;
      background: #ffcc00;
      padding: 5px 6px;
      border-radius: 11px;
    }
  }
  .reg_one {
    background: white;
    height: 435px;
    overflow-y: auto;
    h2 {
      font-family: MicrosoftYaHei;
      font-size: 18px;
      font-weight: normal;
      font-stretch: normal;
      letter-spacing: 2px;
      color: #333333;
      padding: 25px 29px;
    }
    ul {
      .reg_nav {
        padding: 3px 0;
        color: #333333;
        font-size: 18px;
        span {
          text-align: center;
        }
      }
      li {
        padding: 10px 0;
        font-size: 12px;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        span {
          flex: 1;
          text-align: center;
        }
      }
    }
  }
  .reg_two {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    div {
      width: 45%;
      // height: 80%;
      min-height: 425px;
      min-width: 363px;
      box-shadow: 0px 0px 12px 1px rgba(21, 21, 21, 0.23);
    }
    .reg_two_l {
      padding: 24px 17px 0 32px;
      background: white;
      h2 {
        font-size: 19px;
        color: #333333;
        padding: 0 0 8px 0;
        font-weight: normal;
      }
      img {
        width: 223px;
        height: 132px;
      }
      p {
        font-size: 16px;
        color: #333333;
        padding: 6px 0;
        &:last-of-type {
          padding: 13px 0 26px;
        }
      }
    }
    .reg_two_r {
      background: white;
      h2 {
        font-size: 19px;
        font-weight: normal;
        color: #333333;
        padding: 22px 0 0 34px;
      }
      ul {
        .reg_two_r_nav {
          font-size: 16px;
        }
        li {
          display: flex;
          flex-direction: row;
          justify-content: space-between;
          font-size: 12px;
          padding: 13px 0;
          span {
            flex: 1;
            text-align: center;
          }
        }
      }
    }
  }
}
</style>
