<template>
    <div class="card top animated fadeInDown">
        <div class="container-top">

            <div class="left">
                <div id="he-plugin-simple"></div>
            </div>
            <div class="center">
            </div>
            <div class="right">
                <i class="far fa-bookmark" aria-hidden="true" @click="uPopConfig('IndexSet', '快捷导航设置')"></i>
                <i class="far fa-edit" aria-hidden="true" @click="uPopConfig('SearchEngineSet', '搜索引擎设置')"></i>
                <i class="far fa-images" @click="uPopConfig('Style', '主题设置')"></i>
                <i class="far fa-user" @click="uPopConfig('User','用户设置')"></i>

                <!--                <img src="../assets/img/skin.svg" @click="uPopConfig(1)" alt="">-->
                <!--                <img src="../assets/img/user.svg" @click="uPopConfig(2)" alt="">-->
            </div>
        </div>
    </div>
</template>

<script>
    // import SerachBox from "./SearchBox";

    export default {
        name: "AppTop",
        components: {
            // SerachBox
        },
        data() {
            return {
                searchBoxShow: false,
            }
        },
        computed: {
            popConfig() {
                return this.$store.getters.popConfig;
            },
        },
        watch: {},
        mounted() {
            window.addEventListener("scroll", this.scroll);
        },
        methods: {
            uPopConfig(type, title) {

                let show = false;
                if (this.popConfig.componentName === type) {
                    show = !this.popConfig.show;
                } else {
                    show = true;
                }
                this.Utils.pop({
                    show: show,
                    title: title,
                    componentName: type
                });
            },
            scroll() {
                let scrollTop = document.documentElement.scrollTop;
                // window.console.log(scrollTop);
                this.searchBoxShow = scrollTop > 80;
            }
        }

    }
</script>

<style scoped>

    .top {
        z-index: 1;
        position: fixed;
        top: 0;

        border-radius: 0;
        width: 100%;
        /*height: 60px;*/
        /*height: 80px;*/

        display: grid;
    }

    .container-top {
        max-width: 1200px;
        width: 95%;
        justify-self: center;
        align-self: center;

        padding: 10px;

        display: grid;
        grid-template-columns: repeat(3, 1fr);
        /*align-content: center;*/
        /*align-items: center;*/
        /*grid-column-gap: 20px;*/
        /*justify-content: right;*/
        place-items: center;
        place-content: center;

    }


    .left {
        justify-self: left;
    }

    /*.center >>> .row2 {*/
    /*    display: none;*/
    /*}*/


    .right {
        /*grid-area: 2 / 3;*/

        height: 25px;

        justify-self: right;

        /*cursor: pointer;*/

        display: grid;
        grid-template-columns: repeat(4, auto);
        grid-column-gap: 15px;
        place-content: center;
        place-items: center;

        font-size: 20px;
    }

    .right i {
        cursor: pointer;
    }

    img {
        width: 25px;
    }

    /*.right img:last-child {*/
    /*    width: 25px;*/
    /*    height: 25px;*/
    /*}*/

    @media screen and (max-width: 600px) {
    }
</style>