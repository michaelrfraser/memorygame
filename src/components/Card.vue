<template>
    <div>
        <i class="m-0 p-0" :class="iconClass" :style="iconStyle"/>
    </div>
</template>

<script>
    export default {
        name: 'Card',
        props: {
            image: String,
            flipped: Boolean,
            color: String,
        },
        computed: {
            iconClass() {
                if( this.flipped )
                    return "mdi mdi-"+this.image;
                else
                    return "mdi mdi-help-box";
            },
            iconStyle() {
                return {
                    fontSize: '5em',
                    color: this.flipped ? this.color : 'black'
                };
            }
        }
    }
</script>

<style lang="scss">
    @import '~@mdi/font/css/materialdesignicons.css';
    i {
        padding: 10px;
        margin: 10px;
        width: calc(100vw);
        border: 1px solid;
    }
</style>