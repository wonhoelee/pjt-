<script>
import { HorizontalBar } from 'vue-chartjs';
import { fetchAdviseLawyer } from '@/api/advise';

export default {
  extends: HorizontalBar,
  data() {
    return {
      isLoad: false,
      chartData: {
        hoverBackgroundColor: 'red',
        hoverBorderWidth: 10,
        labels: ['교통/운전', '가정', '근로/노동', '부동산', '금융', '정보통신/기술'],
        datasets: [
          {
            backgroundColor: ['rgb(224, 58, 60)', 'rgb(246, 130, 31)', 'rgb(252, 184, 39)', 'rgb(98, 187, 71)', 'rgb(0, 157, 220)', 'rgb(150, 61, 151)'],
            data: [0, 0, 0, 0, 0, 0],
          },
        ],
        hoverOffset: 4,
      },
      values: [],
      uuid: this.lawyer_uuid,
    };
  },
  props: ['lawyer_uuid'],

  watch: {
    lawyer_uuid() {
      this.chart(this.lawyer_uuid);
    },
  },

  methods: {
    async chart(uuid) {
      try {
        const { data } = await fetchAdviseLawyer(uuid);
        for (let i = 0; i < data.length; i++) {
          this.values.push({
            category: data[i].category,
          });
        }
      } catch (err) {}

      for (let index = 0; index < this.values.length; index++) {
        var category = this.values[index].category;
        if (category == '교통/운전') {
          this.chartData.datasets[0].data[0]++;
        } else if (category == '가정') {
          this.chartData.datasets[0].data[1]++;
        } else if (category == '근로/노동') {
          this.chartData.datasets[0].data[2]++;
        } else if (category == '부동산') {
          this.chartData.datasets[0].data[3]++;
        } else if (category == '금융') {
          this.chartData.datasets[0].data[4]++;
        } else if (category == '정보통신/기술') {
          this.chartData.datasets[0].data[5]++;
        }
      }

      // 실제 차트 랜더링 부분
      this.renderChart(this.chartData, {
        borderWidth: '10px',
        hoverBackgroundColor: 'red',
        hoverBorderWidth: '10px',
        maintainAspectRatio: false,
        responsive: true,
        width: '50%',
        height: '50%',

        legend: {
          display: false,
        },
        tooltips: {
          callbacks: {
            label: (tooltipItem) => `${tooltipItem.yLabel}: ${tooltipItem.xLabel}`,
            title: () => null,
          },
        },
      });
    },
  },
};
</script>
