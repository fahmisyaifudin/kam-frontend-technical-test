<template>
  <div>
       <div class="container-fluid">
            <div class="card shadow mb-4">
                <div class="card-header py-3">
                    <h6 class="m-0 font-weight-bold text-primary">Order</h6>
                </div>
                <div class="card-body">
                    <NuxtLink to="/order"><button class="btn btn-primary ml-2 mb-2">Create</button></NuxtLink>
                    <div class="row">
                        <div class="form-group col-md-2">
                            <label for="exampleFormControlSelect1">Region</label>
                            <select class="form-control" id="exampleFormControlSelect1" v-model="fltr.region">
                                <option value="East">East</option>
                                <option value="West">West</option>
                                <option value="Central">Central</option>
                                <option value="South">South</option>
                            </select>
                        </div>

                        <div class="offset-md-5 form-group col-md-2">
                            <label for="exampleFormControlSelect1">Date Start</label>
                            <input type="date" class="form-control" v-model="fltr.dateStart">
                        </div>
                        <div class="form-group col-md-2">
                            <label for="exampleFormControlSelect1">Date Start</label>
                            <input type="date" class="form-control" v-model="fltr.dateEnd">
                        </div>

                        <div class="col-md-1">
                            <button class="btn btn-primary mt-4" @click="filter()">Filter</button>
                        </div>
                    </div>

                    <vuetable ref="vuetable"
                        :api-mode="false"
                        :fields="['order_id', 'date', 'customer_name', 'region', 'product_name', 'sales']"
                        :data="orders"
                        
                    ></vuetable>
                    <div>
                        <button class="btn btn-primary float-right ml-2" @click="nextPage()">Next</button>
                        <button class="btn btn-primary float-right" @click="prevPage()">Prev</button>
                    </div>

                </div>
            </div>
        </div>  
  </div>

</template>

<script>
    import Vuetable from 'vuetable-2'
    
    const qs = require('querystring')

    export default {
        components: { Vuetable },
        data() {
           return {
               orders: [],
               fltr: {
                   dateEnd: null,
                   dateStart: null,
                   region: null
               },
               offset: 0,
               limit: 25,
               sortOrder: [{field: "region", direction: 'desc'}]
           }
        },
        created() {
            this.getOrder(this.limit, this.offset)
        },
        methods: {
            nextPage(){
                 this.offset += this.limit;   
                 this.getOrder(this.limit, this.offset)
            },
            prevPage(){
                if(this.offset > 0){
                    this.offset -= this.limit;   
                    this.getOrder(this.limit, this.offset)
                }
            },
            filter(){
                let input = {
                    region: this.fltr.region,
                    date_start: this.fltr.dateStart,
                    date_end: this.fltr.dateEnd
                }

                fetch("https://api-test.godig1tal.com/order/range_date_region_order", {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/x-www-form-urlencoded',
                    },
                    body: qs.stringify(input)
                })
                .then(res => res.json())
                .then(data => {
                    this.orders = data.data;
                });
            },
            async getOrder(limit, offset){
                const orders = await this.$axios.post('/order/all_order_limit', qs.stringify({ limit, offset }), {
                     headers: {
                        "Content-Type": "application/x-www-form-urlencoded"
                    }
                });

                this.orders = orders.data;
            }
        }
    }

</script>