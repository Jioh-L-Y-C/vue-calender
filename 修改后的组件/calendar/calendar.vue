<style scoped>
/* @media screen and (min-width: 460px) {
  .wh_item_date:hover {
    background: #71c7a5;
    cursor: pointer;
  }
} */
 .wh_item_date:hover {
    background: #666;
    cursor: pointer;
  }
* {
  margin: 0;
  padding: 0;
}

.wh_container {
  /* max-width: 410px; */
  width: 100%;
  margin: auto;
}

li {
  list-style-type: none;
}
.wh_top_changge {
  display: flex;
  background-color: #CBECFF;
  height: 80px;
}

.wh_top_changge li {
  cursor: pointer;
  display: flex;
  color: #333;
  font-size: 18px;
  flex: 1;
  justify-content: center;
  align-items: center;
  /* height: 47px; */
}

.wh_top_changge .wh_content_li {
  cursor: auto;
  flex: 2.5;
}
.wh_content_all {
  font-family: -apple-system, BlinkMacSystemFont, "PingFang SC",
    "Helvetica Neue", STHeiti, "Microsoft Yahei", Tahoma, Simsun, sans-serif;
  background-color: #eee;
  width: 100%;
  overflow: hidden;
  padding-bottom: 20px;
}

.wh_content {
  display: flex;
  flex-wrap: wrap;
  padding: 0 3% 0 3%;
  width: 100%;
}

.wh_content:first-child .wh_content_item_tag,
.wh_content:first-child .wh_content_item {
  color: #ddd;
  font-size: 16px;
}

.wh_content_item,
wh_content_item_tag {
  font-size: 15px;
  width: 13.4%;
  text-align: center;
  color: #fff;
  position: relative;
}
.wh_content_item {
  /* height: 40px; */
}

.wh_top_tag {
  width: 60px;
  height: 60px;
  line-height: 60px;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

.wh_item_date {
     width: 70px;
    height: 70px;
    /* line-height: 80px; */
  margin: auto;
  display: flex;
  /* flex-direction: column; */
  justify-content: center;
  align-items: center;
  display: block;
  line-height: 30px;
  color: #333;
}
.wh_item_date .isorederOk{
  text-align: center;
  color: #71c7a5;

}

.wh_jiantou1 {
  width: 12px;
  height: 12px;
  border-top: 2px solid #ffffff;
  border-left: 2px solid #ffffff;
  transform: rotate(-45deg);
}

.wh_jiantou1:active,
.wh_jiantou2:active {
  border-color: #ddd;
}

.wh_jiantou2 {
  width: 12px;
  height: 12px;
  border-top: 2px solid #ffffff;
  border-right: 2px solid #ffffff;
  transform: rotate(45deg);
}
.wh_content_item > .wh_isMark {
  margin: auto;
  border-radius: 100px;
  background: #6666;
  color: #333;
  display: block;
  z-index: 2;
  position: relative;
}
.wh_content_item > .wh_isMark .nochoose{
  text-align: center;
  }
.wh_content_item .wh_other_dayhide {
  color: #bfbfbf;
}
.wh_content_item .wh_want_dayhide {
  color: #bfbfbf;
  line-height: 80px;
}
.wh_content_item .wh_isToday {
  background: yellow;
  border-radius: 100px;
  display: block;
}
.wh_content_item .wh_isToday .isorederOk{
  text-align: center;
  color: #71c7a5;
}
.wh_content_item .wh_chose_day {
  background: #449D76;
  border-radius: 100px;
  position: relative;
}
/* .wh_content_item .wh_chose_day::after{
  content: "";
  width: 20px;
  height: 20px;
  background-color: red;
  position: absolute;
  top: 0px;
  right: 0;
} */
.wh_content_item .wh_chose_day .chooseDay_icon{
font-size: 22px;
color: red;
position: absolute;
top: -6px;
right: -2px;
}
</style>
<template>
  <section class="wh_container">
    <div class="wh_content_all">
      <div class="wh_top_changge">
        <li @click="PreMonth(myDate,false)">
          <div class="wh_jiantou1"></div>
        </li>
        <li class="wh_content_li">{{dateTop}}</li>
        <li @click="NextMonth(myDate,false)">
          <div class="wh_jiantou2"></div>
        </li>
      </div>
      <div class="wh_content">
        <div class="wh_content_item" v-for="tag in textTop">
          <!-- 星期几 -->
          <div class="wh_top_tag">{{tag}}</div>
        </div>
      </div>
      <div class="wh_content">
        <div class="wh_content_item" v-for="(item,index) in list" @click="clickDay(item,index)">
          <div
            class="wh_item_date"
            v-bind:class="[{ wh_isMark: item.isMark},{wh_other_dayhide:item.otherMonth!=='nowMonth'},{wh_want_dayhide:item.dayHide},{wh_isToday:item.isToday},{wh_chose_day:item.chooseDay},setClass(item)]"
          >{{item.id}}
          <p style="color:#eee" class="nochoose">{{item.isMark ? "已满员":""}}</p>
          <p  class="isorederOk" >{{item.dayHide||item.isMark ? "":"可预约"}}</p>
          <!-- <p v-if="item.chooseDay" style="color:red">{{item.chooseDay}}</p> -->
          <i v-if="item.chooseDay" class="el-icon-success chooseDay_icon" style="color:red"></i>
           </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import timeUtil from "./calendar";
export default {
  
  data() {
    return {
      myDate: [],
      list: [],
      historyChose: [],
      dateTop: ""
    };
  },
  props: {
    markDate: {
      type: Array,
      default: () => []
    },
    markDateMore: {
      type: Array,
      default: () => []
    },
    textTop: {
      type: Array,
      default: () => ["一", "二", "三", "四", "五", "六", "日"]
    },
    sundayStart: {
      type: Boolean,
      default: () => false
    },
    agoDayHide: {
      type: String,
      default: `0`
    },
    futureDayHide: {
      type: String,
      default: `2554387200`
    }
  },
  created() {
    this.intStart();
    this.myDate = new Date();
  },
  methods: {
    intStart() {
      timeUtil.sundayStart = this.sundayStart;
    },
    setClass(data) {
      let obj = {};
      obj[data.markClassName] = data.markClassName;
      return obj;
    },
    clickDay: function(item, index) {
      if (item.otherMonth === "nowMonth" && !item.dayHide) {
        this.getList(this.myDate, item.date);
      }
      if (item.otherMonth !== "nowMonth") {
        item.otherMonth === "preMonth"
          ? this.PreMonth(item.date)
          : this.NextMonth(item.date);
      }
    },
    ChoseMonth: function(date, isChosedDay = true) {
      date = timeUtil.dateFormat(date);
      this.myDate = new Date(date);
      this.$emit("changeMonth", timeUtil.dateFormat(this.myDate));
      if (isChosedDay) {
        this.getList(this.myDate, date, isChosedDay);
      } else {
        this.getList(this.myDate);
      }
    },
    PreMonth: function(date, isChosedDay = true) {
      date = timeUtil.dateFormat(date);
      this.myDate = timeUtil.getOtherMonth(this.myDate, "preMonth");
      this.$emit("changeMonth", timeUtil.dateFormat(this.myDate));
      if (isChosedDay) {
        this.getList(this.myDate, date, isChosedDay);
      } else {
        this.getList(this.myDate);
      }
    },
    NextMonth: function(date, isChosedDay = true) {
      date = timeUtil.dateFormat(date);
      this.myDate = timeUtil.getOtherMonth(this.myDate, "nextMonth");
      this.$emit("changeMonth", timeUtil.dateFormat(this.myDate));
      if (isChosedDay) {
        this.getList(this.myDate, date, isChosedDay);
      } else {
        this.getList(this.myDate);
      }
    },
    forMatArgs: function() {
      let markDate = this.markDate;
      let markDateMore = this.markDateMore;
      markDate = markDate.map(k => {
        return timeUtil.dateFormat(k);
      });
      markDateMore = markDateMore.map(k => {
        k.date = timeUtil.dateFormat(k.date);
        return k;
      });
      return [markDate, markDateMore];
    },
    getList: function(date, chooseDay, isChosedDay = true) {
      const [markDate, markDateMore] = this.forMatArgs();
      this.dateTop = `${date.getFullYear()}年${date.getMonth() + 1}月`;
      let arr = timeUtil.getMonthList(this.myDate);
      for (let i = 0; i < arr.length; i++) {
        let markClassName = "";
        let k = arr[i];
        k.chooseDay = false;
        const nowTime = k.date;
        const t = new Date(nowTime).getTime() / 1000;
        //看每一天的class
        for (const c of markDateMore) {
          if (c.date === nowTime) {
            markClassName = c.className || "";
          }
        }
        //标记选中某些天 设置class
        k.markClassName = markClassName;
        k.isMark = markDate.indexOf(nowTime) > -1;
        //无法选中某天
        k.dayHide = t < this.agoDayHide || t > this.futureDayHide||t==Date.parse(this.markDate)/1000
        // console.log(Date.parse(this.markDate)/1000,"无法选择的日期",t)
        if (k.isToday) {
          this.$emit("isToday", nowTime);
        }
        let flag = !k.dayHide && k.otherMonth === "nowMonth";
        if (chooseDay && chooseDay === nowTime && flag) {
          this.$emit("choseDay", nowTime);
          this.historyChose.push(nowTime);
          k.chooseDay = true;
        } else if (
          this.historyChose[this.historyChose.length - 1] === nowTime &&
          !chooseDay &&
          flag
        ) {
          k.chooseDay = true;
        }
      }
      this.list = arr;
    },
  },
  mounted() {
    this.getList(this.myDate);
  },
  watch: {
    markDate: {
      handler(val, oldVal) {
        this.getList(this.myDate);
      },
      deep: true
    },
    markDateMore: {
      handler(val, oldVal) {
        this.getList(this.myDate);
      },
      deep: true
    },
    agoDayHide: {
      handler(val, oldVal) {
        this.getList(this.myDate);
      },
      deep: true
    },
    futureDayHide: {
      handler(val, oldVal) {
        this.getList(this.myDate);
      },
      deep: true
    },
    sundayStart: {
      handler(val, oldVal) {
        this.intStart();
        this.getList(this.myDate);
      },
      deep: true
    }
  }
};
</script>