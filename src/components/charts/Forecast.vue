<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref, computed } from 'vue';
import { useMapStore } from '../../store/mapStore';

const props = defineProps(['chart_config', 'activeChart', 'series', 'map_config']);
const mapStore = useMapStore();

const chartOptions = ref({
	chart: {
		height:350,
		type:'line',
		toobar:{
			show:false
		}
	},
	colors:['#0f0'],
	chart: {
    // foreColor: '#f00',
},
	x: {
          show: true,
          format: 'yyyy',
          formatter: undefined,
      },
	forecastDataPoints: {
              count: 7
            },
	grid: {
		show: true,
	},
	legend: {
		show: true,
	},
	tooltip: {
		// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			return '<div class="chart-tooltip">' +
				'<h6>' + w.globals.labels[dataPointIndex] + `${props.chart_config.categories ? '-' + w.globals.seriesNames[seriesIndex] : ''}` + '</h6>' +
				'<span>' + series[seriesIndex][dataPointIndex] + '</span>' +
				'</div>';
		},
	},
			title: {
              text: 'Forecast',
              align: 'left',
              style: {
                fontSize: "16px",
                color: '#666'
              }
            },
			fill: {
              type: 'gradient',
              gradient: {
                shade: 'dark',
                gradientToColors: [ '#F5DEB3'],
                shadeIntensity: 1,
                type: 'horizontal',
                opacityFrom: 1,
                opacityTo: 1,
                stops: [0, 100, 100, 100]
              },},
	stroke: {
		colors: ['#282a2c'],
		show: true,
		width: 3,
	},
	xaxis: {
		axisBorder: {
			show: false,
		},
		axisTicks: {
			show: false,
		},
		labels: {
			show: false,
		},
	},


});

function handleDataSelection(e, chartContext, config) {
	if (!props.chart_config.map_filter) {
		return;
	}
	if (config.dataPointIndex !== selectedIndex.value) {
		mapStore.addLayerFilter(`${props.map_config[0].index}-${props.map_config[0].type}`, props.chart_config.map_filter[0], props.chart_config.map_filter[1][config.dataPointIndex]);
		selectedIndex.value = config.dataPointIndex;
	} else {
		mapStore.clearLayerFilter(`${props.map_config[0].index}-${props.map_config[0].type}`);
		selectedIndex.value = null;
	}
}

</script>

<template>
	<div v-if="activeChart === 'Forecast'">
		<apexchart width="100%" :height="350" type="line" :options="chartOptions" :series="series"
			@dataPointSelection="handleDataSelection"></apexchart></div>
</template>