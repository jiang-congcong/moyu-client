<template>
    <div>
        <div style="display: flex; height: 50%;">
            <div style="flex: 1; width: 50%;">
                <canvas id="userStatistic"></canvas>
            </div>
            <div style="flex: 1; width: 50%; margin-left: 30px;">
                <canvas id="productStatistic"></canvas>
            </div>
        </div>

        <div style="display: flex; height: 50%; margin-top: 30px;">
            <div style="flex: 1; width: 50%;">
                <canvas id="typeStatistic"></canvas>
            </div>
            <div style="flex: 1; width: 50%; margin-left: 30px;">
                <canvas id="incomeStatistic"></canvas>
            </div>
        </div>

    </div>
</template>


<script>
import Chart from 'chart.js';

export default {
    data() {
        return {

        }
    },
    //页面加载好后触发的操作
    mounted() {
        var userStatistic = document.getElementById("userStatistic");
        new Chart(userStatistic, {
            type: 'bar', //柱状图
            data: {
                labels: ["Red", "Blue", "Yellow", "Green", "Purple", "Orange"],
                datasets: [{
                    label: '用户统计',
                    data: [12, 19, 3, 5, 2, 3],
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255,99,132,1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: true
                        }
                    }]
                }
            }
        });

        var productStatistic = document.getElementById("productStatistic");
        new Chart(productStatistic, {
            type: 'line', //曲线图
            data: {
                labels: ["娃哈哈", "茶颜悦色", "卫龙", "三只松鼠", "良品铺子", "洁柔"],
                datasets: [{
                    label: '品牌销量',
                    data: [12, 19, 3, 5, 2, 3],
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255,99,132,1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: true
                        }
                    }]
                }
            }
        });

        var typeStatistic = document.getElementById("typeStatistic");
        new Chart(typeStatistic, {
            type: 'pie', //饼状图
            data: {
                labels: ["蔬菜", "水果", "五金", "调味品", "日用品", "快消品"],
                datasets: [{
                    label: '分类下商品类型数量汇总',
                    data: [30, 26, 16, 49, 100, 320],
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255,99,132,1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: true
                        }
                    }]
                }
            }
        });

        var incomeStatistic = document.getElementById("incomeStatistic");
        new Chart(incomeStatistic, {
            type: 'doughnut', //环形图
            data: {
                labels: ["1月", "2月", "3月", "4月", "5月", "6月"],
                datasets: [{
                    //label: '收入月份统计/万元',
                    data: [300, 320, 360, 330, 420, 500],
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255,99,132,1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: true
                        }
                    }]
                }
            }
        });
    },

    methods: {
        
    }
}
</script>

<style></style>