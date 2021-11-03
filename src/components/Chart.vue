<template>
                <div class="legend__chart">
                    <PieChart ref="chart" />
                </div>
</template>

<script>

import legend from "@/assets/data/legend.json";
import { Doughnut as PieChart } from "vue-chartjs";
import tables from "@/assets/data/tables.json";

export default {
    components: {
        PieChart
    },
    data() {
        return {
            legend: [],
            actualCounter: []
        };
    },
    created() {
        this.loadLegend();
    },
    mounted() {
        this.tables = tables;
        this.getActualCounter();
        this.makeChart();
    },
    methods: {
        getActualCounter() {
            const result = [0];
            this.tables.forEach((table) => {
                if(!result[table.group_id]) {
                    result[table.group_id] = 1;
                } else {
                    result[table.group_id] += 1;
                }
            });
           this.actualCounter = result;
        },
        loadLegend() {
            this.legend = legend;
        },

        makeChart() {
            const chartData = {
                labels: this.legend.map((it) => it.text),
                datasets: [
                    {
                        label: "Легенда",
                        backgroundColor: this.legend.map(
                            (legendItem) => legendItem.color
                        ),

                        data: this.actualCounter
                    },
                ],
            };
            const options = {
                borderWidth: "10px",
                legend: {
                    display: false,
                },
            };
            this.$refs.chart.renderChart(chartData, options);
        }
    },
};
</script>

<style scoped>

</style>
