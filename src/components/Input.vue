<template>
     <label :for="name">{{label}}</label>
     <input :type="type" :name="name" class="form-control" :class="{'is-invalid':errors[name]}" />
     <div class="invalid-feedback" v-if="errors[name]">{{errors[name][0]}}</div>
</template>

<script>
    export default {
        props: {
            label: {
                type: String,
                default: "input label"
            },
            name:{
                type: String,
                default:"input name"
            },
            type:{
                type: String,
                default:"text"
            },
            errors:{
                type:Object,
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>