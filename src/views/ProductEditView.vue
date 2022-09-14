<template>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <h1>Product Edit</h1>
                <hr>
                <form @submit.prevent="updateProduct" ref="productEdit">
                    <div class="row">
                        <div class="col">
                            <div class="mb-3">
                               <Input label="ProductName" :value="product.name" name="name" type="text" :errors="errors"/>
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                               <Input label="Price" :value="product.price" name="price" type="number" :errors="errors"/>
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                               <Input label="Stock" :value="product.stock" name="stock" type="number" :errors="errors"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-12 text-center">
                        <button :disabled="isLoading" class="btn btn-primary">
                            <span v-if="isLoading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                            Update
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import Input from '@/components/Input.vue'
import axios from 'axios';
import Swal from 'sweetalert2';
import { mapGetters } from 'vuex';
    export default {
        data() {
            return {
                errors: {},
                isLoading:false,
                product:{}
            }
        },
        components: { Input },
        computed: {
            ...mapGetters([
                'getUrl'
            ])
        },
        methods: {
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
            updateProduct() {
                this.isLoading = true;
                let formData = new FormData(this.$refs.productEdit);
                axios.put(this.getUrl('/products/'+this.$route.params.id),new URLSearchParams(formData).toString())
                .then(res => {
                    
                    if(res.data.success === true){
                        this.errors = {}
                        this.product = res.data.product;
                       this.showToast('success',res.data.message);
                       this.$router.push('/product');
                    }
                })
                .catch(e=>{
                            console.log(e)
                        this.errors = e.response.data.errors;
                        this.showToast('error',e.response.data.message);
                    
                })
                .finally(_=>this.isLoading=false)
            },
            fetchProduct(id) {
                axios.get(this.getUrl('/products/'+id))
                .then(res =>{
                    this.product = res.data.data;
                })
                .catch(e=>console.log(e))
            },
        },
        mounted(){
            this.fetchProduct(this.$route.params.id);
        }
    }
</script>

<style lang="scss" scoped>

</style>