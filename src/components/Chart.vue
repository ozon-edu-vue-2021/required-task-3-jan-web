<template>
                <div class="legend__chart">
                    <PieChart ref="chart" />
                </div>
</template>

<script>
import { Doughnut as PieChart } from "vue-chartjs";
export default {
    components: {
        PieChart
    },
        props: {
            tables: {
                type: Array,
                default: null,
            },
            legend: {
                type: Array,
                default: null,
            },
    },
    data() {
        return {
            actualCounter: []
        };
    },
    mounted() {
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
