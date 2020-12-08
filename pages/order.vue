<template>
  <div>
       <div class="container-fluid">
            <div class="card shadow mb-4">
                <div class="card-header py-3">
                    <h6 class="m-0 font-weight-bold text-primary">Order</h6>
                </div>
                <div class="card-body">
                        <div class="form-group">
                            <label>Customer</label>
                            <v-select label="customer_name" :reduce="customer => customer.customer_id" :options="customers" v-model="input.customer_id"></v-select>
                        </div>
                         <div class="form-group">
                            <label>Product</label>
                            <v-select label="product_name" :reduce="product => product.product_id"  :options="products" v-model="input.product_id"></v-select>
                        </div>
                        <div class="form-group">
                            <label for="exampleFormControlSelect1">Region</label>
                            <select v-model="input.region" class="form-control" id="exampleFormControlSelect1">
                                <option value="East">East</option>
                                <option value="West">West</option>
                                <option value="Central">Central</option>
                                <option value="South">South</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>Date</label>
                             <input type="date" class="form-control" v-model="input.date">
                        </div>
                         <div class="form-group">
                            <label>Sales</label>
                            <input type="number" step='0.01' class="form-control" v-model="input.sales">
                        </div>
                        <button class="btn btn-primary float-right" @click="actionSave()">Save</button>
                </div>
            </div>
        </div>  
  </div>

</template>

<script>
    import vSelect from 'vue-select'
    import 'vue-select/dist/vue-select.css';
    import moment from 'moment';
    const qs = require('querystring')

    export default {
        components: { vSelect },
        data() {
            return {
                input: {
                    customer_id: null,
                    product_id: null,
                    sales: 0.00,
                    region: null,
                    date: null
                },
                customers: [],
                products: []
            }
        },
        created() {
            this.getCustomer();
            this.getProduct();
        },
        methods: {
            async getCustomer(){
                const customers = await this.$axios.$get('/customer/all_customer')
                this.customers = customers.data;
            },
            async getProduct(){
                const products = await this.$axios.$get('/product/all_product')
                this.products = products.data;
            },
            actionSave(){
                const randInt = (min, max) => {
                    var range = max - min;
	
                    var rand = Math.floor(Math.random() * (range + 1));
                    return min + rand;
                }
                let data = {
                    order_id: 'US-' + moment(this.input.date).format('YYYY') + '-' + randInt(600000, 799999),
                    ...this.input
                }
                
                this.$axios.$post('/order/new_order', qs.stringify(data), {
                    headers: {
                        "Content-Type": "application/x-www-form-urlencoded"
                    }
                }).then(res => {
                    console.log(res);
                    if(res.status == "200 - success"){
                        this.$swal(
                            "Success!",
                            res.data.msg,
                            "success"
                        ).then(() => {
                             this.$router.push('/list-order')
                        });
                    }
                })
            }
        }
    }

</script>
