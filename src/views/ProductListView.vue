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
                        <tr v-for="row in rows.data" :key="row">
                            <td>{{row.id}}</td>
                            <td>{{row.name}}</td>
                            <td>{{row.price}}</td>
                            <td>{{row.stock}}</td>
                            <td>{{row.date}}</td>
                            <td>{{row.time}}</td>
                            <td>
                                <button 
                                class="btn btn-sm btn-outline-primary ms-2"
                                 @click="fetchProduct(row.id)"
                                 >
                                    <i class="bi bi-info"/>
                                </button>
                                <button 
                                    class="btn btn-sm btn-outline-danger ms-2"
                                    @click="deleteProduct(row.id)"
                                >
                                    <i class="bi bi-trash3"/>
                                </button>
                                <router-link :to="'product/edit/'+row.id" class="btn btn-sm btn-outline-warning ms-2">
                                    <i class="bi bi-pencil-square"/>
                                </router-link>
                            </td>
                        </tr>
                    </tbody>
                </table>
                <Pagination v-if="rows.meta" :links="rows.meta.links" @fetchLink = "fetchProducts"/>
                <Model v-if="isShowModel" :product="product"/>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { mapGetters } from 'vuex'
import Pagination from '@/components/Pagination.vue'
    export default {
  components: { Pagination },
        data() {
            return {
                rows: {},
                product : {},
            }
        },
        computed: {
            ...mapGetters([
                'getUrl'
            ])
        },
        methods: {
            fetchProducts(url) {
                axios.get(url)
                .then(res =>{
                    this.rows = res.data
                })
                .catch(e=>console.log(e))
            },
            fetchProduct(id) {
                axios.get(this.getUrl('/products/'+id))
                .then(res =>{
                    this.product = res.data.data;
                    console.log(this.product);
                })
                .catch(e=>console.log(e))
            },
            deleteProduct(id) {
                axios.delete(this.getUrl('/products/')+id)
                .then(res => {
                    if(res.data.success == true){
                        this.fetchProducts(this.rows.meta.links.find(link => link.active === true).url)
                    }
                })
            },
            showToast(icon,message) {
                const Toast = Swal.mixin({
                                toast: true,
                                position: 'top-end',
                                showConfirmButton: false,
                                timer: 3000,
                                timerProgressBar: true,
                                didOpen: (toast) => {
                                    toast.addEventListener('mouseenter', Swal.stopTimer)
                                    toast.addEventListener('mouseleave', Swal.resumeTimer)
                                }
                                })

                                Toast.fire({
                                icon: icon,
                                title: message
                                })
            
            },
        },
        mounted(){
            this.fetchProducts(this.getUrl('/products'));
        }
    }
</script>

<style lang="scss" scoped>

</style>