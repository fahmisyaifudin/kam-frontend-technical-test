<template>
  <div class="container-fluid">
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">Dashboard</h1>
    </div>
     <div class="row">
        <div class="form-group col-md-2">
            <label for="exampleFormControlSelect1">Region</label>
            <select class="form-control" id="exampleFormControlSelect1" v-model="region">
                <option :value="null">All</option>
                <option value="East">East</option>
                <option value="West">West</option>
                <option value="Central">Central</option>
                <option value="South">South</option>
            </select>
        </div>

        <div class="offset-md-7 form-group col-md-2">
            <label for="exampleFormControlSelect1">Year</label>
             <select class="form-control" id="exampleFormControlSelect1" v-model="year">
                 <option :value="null">All</option>
                <option :value="year" v-for="(year, index) in years" :key="index">{{ year }}</option>
            </select>
        </div>

        <div class="col-md-1">
            <button class="btn btn-primary mt-4" @click="filter()">Filter</button>
        </div>
    </div>
     <h5>Customers with the most purchases</h5>
     <div class="row">
        <!-- Earnings (Monthly) Card Example -->
       
        <div class="col-xl-4 col-md-4 mb-4" v-for="(customer, index) in topCustomers" :key="index">
            <div class="card border-left-primary shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-md font-weight-bold text-primary text-uppercase mb-1">
                                {{ customer.customer_name }}</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">${{ round(customer.total_buy) }}</div>
                        </div>
                        <div class="col-auto">
                            <i class="fas fa-calendar fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
    </div>
    <div class="row">
         <!-- Content Column -->
      <div class="col-lg-6 mb-4">

          <!-- Project Card Example -->
          <div class="card shadow mb-4">
              <div class="card-header py-3">
                  <h6 class="m-0 font-weight-bold">Top Products</h6>
              </div>
              <canvas id="topProductChart" width="200" height="100"></canvas>
              <div class="card-body">
                  <h4 class="small font-weight-bold text-primary">Products <span
                          class="float-right text-primary">Total Sold</span></h4>
                  <hr>
                  <h4 class="small font-weight-bold" v-for="(product, index) in topProducts" :key="index">{{ index + 1}} - {{ product.product_name }}<span
                          class="float-right">{{ product.total_sold }}</span></h4>
              </div>
          </div>

      </div>
      <div class="col-lg-6 mb-4">

          <!-- Project Card Example -->
          <div class="card shadow mb-4">
              <div class="card-header py-3">
                  <h6 class="m-0 font-weight-bold">Worst Products</h6>
              </div>
              <canvas id="worstProductChart" width="200" height="100"></canvas>
              <div class="card-body">
                  <h4 class="small font-weight-bold text-primary">Products <span
                          class="float-right text-primary">Total Sold</span></h4>
                  <hr>
                  <h4 class="small font-weight-bold" v-for="(product, index) in worstProducts" :key="index">{{ index + 1}} - {{ product.product_name }}<span
                          class="float-right">{{ product.total_sold }}</span></h4>
              </div>
          </div>

      </div>
    </div>
  </div>
</template>

<script>

    import 'chart.js/dist/Chart.bundle.min.js';
    import 'chart.js/dist/Chart.min.css';
    const qs = require('querystring')

    export default {
        data() {
           return {
              region: null,
              year: null,
              topProducts: [],
              worstProducts: [],
              topCustomers: [],
              years: [2020, 2019, 2018, 2017, 2016, 2015, 2014, 2013, 2012, 2011, 2010],
              topProductChart: null,
              worstProductChart: null
           }
        },
        created() {
            this.getDashboard(this.region, this.year)
        },
        mounted(){
            var ctxTop = document.getElementById('topProductChart');
            var ctxWorst = document.getElementById('worstProductChart');
            let data = {
                type: 'bar',
                data: {
                    labels: [],
                    datasets: [{
                        label: 'Total Sold',
                        data: [],
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.2)',
                            'rgba(54, 162, 235, 0.2)',
                            'rgba(255, 206, 86, 0.2)',
                            'rgba(75, 192, 192, 0.2)',
                            'rgba(153, 102, 255, 0.2)',
                            'rgba(255, 159, 64, 0.2)',
                            'rgba(28, 21, 214, 0.2)',
                            'rgba(7, 122, 61, 0.2)',
                            'rgba(138, 138, 138, 0.2)',
                            'rgba(245, 59, 111, 0.2)'
                        ],
                        borderColor: [
                            'rgba(255, 99, 132, 1)',
                            'rgba(54, 162, 235, 1)',
                            'rgba(255, 206, 86, 1)',
                            'rgba(75, 192, 192, 1)',
                            'rgba(153, 102, 255, 1)',
                            'rgba(255, 159, 64, 1)',
                            'rgba(28, 21, 214, 1)',
                            'rgba(7, 122, 61, 1)',
                            'rgba(138, 138, 138, 1)',
                            'rgba(245, 59, 111, 1)'
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
            }
            this.topProductChart = new Chart(ctxTop, data);
            this.worstProductChart = new Chart(ctxWorst, data);
        },
        methods: {
            filter(){
                this.getDashboard(this.region, this.year)
            },
            round(number){
              return parseFloat(number).toFixed(2)
            },
            getDashboard(region, year){
              this.$axios.post("/order/top_10_product", qs.stringify({
                region,
                year
              })).then(res => {
                this.topProducts = res.data.data
                let key = [], value = [];
                res.data.data.forEach((element, index) => {
                    key.push(index + 1);
                    value.push(element.total_sold);
                });
                this.topProductChart.data.labels = key;
                this.topProductChart.data.datasets[0].data = value;
                this.topProductChart.update();
              })

              this.$axios.post("/order/worst_10_product", qs.stringify({
                region,
                year
              })).then(res => {
                this.worstProducts = res.data.data
                let key = [], value = [];
                res.data.data.forEach((element, index) => {
                    key.push(index + 1);
                    value.push(element.total_sold);
                });
                this.worstProductChart.data.labels = key;
                this.worstProductChart.data.datasets[0].data = value;
                this.worstProductChart.update();
              })

              this.$axios.post("/order/customer_ot_year", qs.stringify({
                region,
                year
              })).then(res => {
                this.topCustomers = res.data.data
              })

            }
        }
    }

</script>
