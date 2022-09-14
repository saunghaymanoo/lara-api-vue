<template>
    <div class="input-group w-25">
        <input type="text" name="keyword" v-model="keyword" class="form-control">
        <button @click="search" class="btn btn-primary">
             <i class="bi bi-search"></i>
        </button>
    </div>
</template>

<script>
import throttle from 'lodash/throttle'
import debounce from 'lodash/debounce'
    export default {
        data() {
            return {
                keyword: null
            }
        },
        methods: {
            search:throttle(function() {
               this.$emit('search',this.keyword); 
            },500)
            
        },
        watch:{
            keyword:debounce(function(){
                 this.$emit('search',this.keyword);
            },500)
        }
    }
</script>

<style lang="scss" scoped>

</style>