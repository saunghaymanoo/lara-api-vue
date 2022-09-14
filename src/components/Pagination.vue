<template>
    <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li :key="link" v-for="link in links" class="page-item">
                <a 
                class="page-link"
                :class="{'active':link.active,'disabled':!link.url}" 
                href="#"
                @click="go(link.url)"
                >
                {{link.label}}
                </a>
            </li>
        </ul>
</nav>
</template>

<script>
    export default {
        props: {
            links: {
                type: Array,
            },
        },
        methods: {
            linkHtml() {
                this.links[0].label = "prev";
                this.links[this.links.length-1].label = "next";
            },
            go(url){
                this.$emit('fetchLink',url);
            }
        },
        mounted(){
            this.linkHtml();
        },
        updated(){
            this.linkHtml();
        }
    }
</script>

<style lang="scss" scoped>

</style>