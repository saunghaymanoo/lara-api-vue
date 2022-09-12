<template>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <h1>Product Create</h1>
                <hr>
                <form @submit.prevent="addProduct" ref="productCreate">
                    <div class="row">
                        <div class="col">
                            <div class="mb-3">
                               <Input label="ProductName" name="name" type="text" :errors="errors"/>
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                               <Input label="Price" name="price" type="number" :errors="errors"/>
                            </div>
                        </div>
                        <div class="col">
                            <div class="mb-3">
                               <Input label="Stock" name="stock" type="number" :errors="errors"/>
                            </div>
                        </div>
                    </div>
                    <div class="col-12">
                        <div class="mb-4">
                            <label for="photos" class="form-label">Product Photo</label>
                            <input type="file" class="form-control" id="photos" name="photos[]" multiple :class="{'is-invalid':errors.photos}">
                            <div class="invalid-feedback" v-if="errors.photos">{{errors.photos[0]}}</div>
                        </div>
                    </div>
                    <div class="col-12 text-center">
                        <button class="btn btn-primary">Create</button>
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
                errors: {}
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
            addProduct() {
                let formData = new FormData(this.$refs.productCreate);
                axios.post(this.getUrl('/products'),formData)
                .then(res => {
                    if(res.data.success === true){
                        this.errors = {}
                        this.$refs.productCreate.reset()

                       this.showToast('success',res.data.message);
                    }
                })
                .catch(e=>{
                    console.log(e)
                    if(e.response.status == 422){
                        this.errors = e.response.data.errors;
                        this.showToast('error',e.response.data.message);
                    }
                    
                })
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>