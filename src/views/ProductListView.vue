<template>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <h1>Product List</h1>
                <hr>
                <table class="table table-striped text-end align-middle">
                    <thead>
                        <th>#</th>
                        <th>Product Name</th>
                        <th>Price</th>
                        <th>Stock</th>
                        <th>Date</th>
                        <th>Time</th>
                        <th>Control</th>
                    </thead>
                    <tbody>
                        <tr v-for="row in rows" :key="row">
                            <td>{{row.id}}</td>
                            <td>{{row.name}}</td>
                            <td>{{row.price}}</td>
                            <td>{{row.stock}}</td>
                            <td>{{row.date}}</td>
                            <td>{{row.time}}</td>
                            <td>
                                <button class="btn btn-sm btn-outline-primary ms-2">
                                    <i class="bi bi-info"/>
                                </button>
                                <button class="btn btn-sm btn-outline-danger ms-2">
                                    <i class="bi bi-trash3"/>
                                </button>
                                <button class="btn btn-sm btn-outline-warning ms-2">
                                    <i class="bi bi-pencil-square"/>
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { mapGetters } from 'vuex'
    export default {
        data() {
            return {
                rows: {}
            }
        },
        computed: {
            ...mapGetters([
                'getUrl'
            ])
        },
        methods: {
            fetchProducts() {
                axios.get(this.getUrl('/products'))
                .then(res =>{ 
                    this.rows = res.data.data
                })
                .catch(e=>console.log(e))
            }
        },
        mounted(){
            this.fetchProducts();
        }
    }
</script>

<style lang="scss" scoped>

</style>