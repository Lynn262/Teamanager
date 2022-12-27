<template>
  <div class="x-bar">
    <!-- <div id="gantt" :option="option" style="width: 100%"></div> -->
    <highcharts :constructorType="'ganttChart'" class="hc" :options="option" :callback="afterChartCreate" ref="chart" :highchart="hcInstance"></highcharts>
  </div>
</template>

<script>
import Highcharts from "highcharts";
import gantt from "highcharts/modules/gantt";
import noDataToDisplay from "highcharts/modules/no-data-to-display";
import {Chart} from "highcharts-vue"

gantt(Highcharts);
noDataToDisplay(Highcharts);

export default {
  components:{
    highcharts : Chart
  },
  name: "GanttChart",
  props: {
    width: {
      type: Number,
    },
    height: {
      type: Number,
    },
    option: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      chart: {},
      hcInstance:Highcharts
    };
  },
  mounted() {
    // this.$nextTick(() => {
    //   this.chart = Highcharts.chart("gantt", this.option);
    // });
  },
  methods:{
    afterChartCreate(chart){
      this.chart = chart;
      console.log("afterChartCreate");
      this.option.chart.height = this.height;
      this.chart.setSize(this.width, this.height, true);
    }
  },
  watch: {
    width(newValue, oldValue) {
      this.$refs.chart.chart.setSize(newValue, this.height, true);
    },
    height(newValue, oldValue) {
      this.$refs.chart.chart.setSize(this.width, newValue, true);
    },
    // option() {
    //   let length = this.chart.series.length;
    //   for (let i = 0; i < length; i++) {
    //     this.chart.series[i].setData(this.option.series[i].data);
    //   }
    //   //替换完毕后重绘，调用api中的redraw
    //   this.chart.redraw();
    // },
  },
};
</script>

<style scoped>
.x-bar {
  margin-bottom: 1rem;
}
</style>
