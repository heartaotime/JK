<template>
    <transition enter-active-class="animated fadeInUp faster" leave-active-class="animated fadeOutDown faster">
        <div class="dialog-module" v-show="show">
            <div class="popcommon-module" :style="styleSet">
                <div class="popcommon-content">
                    <header>
                        <slot name="header">弹窗标题</slot>
                        <div @click="close()" name="dialog-close">
                            <i class="fas fa-times" aria-hidden="true"></i>
                        </div>
                    </header>
                    <section>
                        <slot name="main">弹窗内容</slot>
                    </section>
                    <!--                    <footer>-->

                    <!--                    </footer>-->
                </div>
            </div>
        </div>
    </transition>
</template>

<script>

    export default {
        name: "pop",
        components: {},
        props: {
            show: {
                type: Boolean,
                default: false,
                required: true
            },
            maxWidth: {
                type: String,
                default: '800px'
            },
            maxHeight: {
                type: String,
                default: 'auto'
            },
            styleSet: {
                type: Object,
                default: function () {
                    return {};
                }
            }
        },
        data() {
            return {}
        },
        computed: {},
        watch: {
            show() {
                if (this.show) {
                    document.querySelector('#search-form').style.zIndex = '0';
                } else {
                    document.querySelector('#search-form').style.zIndex = '10';
                }
            }
        },
        mounted() {
        },
        methods: {
            close() {
                this.$emit("close");
            }
        }
    }
</script>

<style scoped lang="scss">

    .dialog-module {
        z-index: 20;
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
    }

    .popcommon-module {
        position: absolute;
        top: 30%;
        left: 0;
        right: 0;
        margin: 0 auto;
        width: 98vw;
        max-width: 800px;
        max-height: auto;
    }


    .popcommon-content {
        box-shadow: 0 0 18px rgba(70, 70, 40, .255);
        border-radius: 5px;
        background-color: rgba(255, 255, 255, 1);

        /*padding: 20px;*/
        /*width: 800px;*/
        /*height: 600px;*/

        /*max-width: 800px;*/
        /*width: 98vw;*/

        margin: 0 auto;

        display: grid;
        grid-template-columns: 1fr;
        align-content: start;
        justify-content: stretch;
        align-items: center;
    }

    header {
        $height: 50px;
        line-height: $height;
        height: $height;

        padding-left: 10px;
        border-bottom: 1px solid #ededed;
        font-weight: bold;

        letter-spacing: 1px;

        display: grid;
        grid-template-columns: 1fr $height;
        justify-content: space-between;

        background-color: #e0e0e0;
        border-color: #e0e0e0;
        border-radius: 5px 5px 0 0;

        [name='dialog-close'] {
            cursor: pointer;
            text-align: center;
        }

    }


    section {
        display: grid;
        grid-template-columns: 1fr;
        padding: 1rem;
    }

    @media screen and (max-width: 600px) {
        /*.popcommon-module {*/
        /*    top: 50px;*/
        /*}*/

        /*.popcommon-module {*/
        /*    position: fixed;*/
        /*}*/

        /*.popcommon-content {*/
        /*    width: 98%;*/
        /*}*/
    }

</style>