<template>
    <div class="module">

        <div class="tab">
            <div class="item" v-for="(item, index) in logoShowTypes" :key="item.code">
                <input type="radio" :id="'radio' + index" name="logoShowType" :value="item.code"
                       v-model="logoShowType"/>
                <label :for="'radio' + index" :title="item.desc">{{item.name}}</label>
            </div>
        </div>

        <div class="row">

            <div class="img" v-show="logoShowType === 'url'" @click="$refs.upload.click()">
                <i class="fas fa-plus" aria-hidden="true" v-show="!url || url == ''"></i>
                <img :src="url" v-show="url && url !== ''">
            </div>

            <div class="time" v-show="logoShowType === 'time'">
                <span>{{time}}</span>
            </div>

            <div class="weather" v-show="logoShowType === 'weather'">
                <Weather></Weather>
            </div>

        </div>


        <upload ref="upload" @upLoadSuccess="(res) => {url = res}"></upload>

        <div class="btn-group">
            <btn @click.native="save()" height="40px">保存</btn>
        </div>


    </div>
</template>

<script>
    import Weather from "./Weather";

    window.WIDGET = {
        CONFIG: {
            "modules": "01234",
            "background": 5,
            "tmpColor": "4A4A4A",
            "tmpSize": "15",
            "cityColor": "4A4A4A",
            "citySize": "15",
            "aqiSize": "15",
            "weatherIconSize": "20",
            "alertIconSize": "16",
            "padding": "0px 0px 0px 0px",
            "shadow": "0",
            "language": "auto",
            "borderRadius": 5,
            "fixed": "false",
            "vertical": "middle",
            "horizontal": "left",
            "key": "a57958f86f70480d988820933f1d0f87"
        }
    }
    // window.onload = function () {
    //     document.querySelector('#he-plugin-simple').style.zIndex = 2;
    // }
    export default {
        components: {Weather}
    }
</script>
<script src="https://widget.heweather.net/simple/static/js/he-simple-common.js?v=1.1"></script>
<script>
    import Weather from "./Weather";

    export default {
        name: "LogoSet",
        components: {
            Weather
        },
        data() {
            return {
                url: '',
                logoShowType: '',
                logoShowTypes: [
                    {
                        code: 'none',
                        name: '无Logo',
                        desc: '不展示Logo'
                    },
                    {
                        code: 'url',
                        name: '图片',
                        desc: '自己上传一张图片，或者填入一个图片的链接'
                    },
                    {
                        code: 'time',
                        name: '时间',
                        desc: '每分钟更新'
                    },
                    {
                        code: 'weather',
                        name: '天气',
                        desc: '天气'
                    },

                ],
                time: ''
            }
        },
        computed: {
            openUserInfo() {
                return JSON.parse(JSON.stringify(this.$store.getters.openUserInfo));
            }
        },
        watch: {
            openUserInfo: {
                handler() {
                    this.init();
                },
                deep: true
            },
        },
        mounted() {
            this.init();
            setInterval(() => {
                let now = new Date();
                let hours = now.getHours() > 9 ? now.getHours() : '0' + now.getHours();
                let minutes = now.getMinutes() > 9 ? now.getMinutes() : '0' + now.getMinutes();
                // let seconds = now.getSeconds() > 9 ? now.getSeconds() : '0' + now.getSeconds();
                this.time = hours + ':' + minutes;
            }, 1000);
        },
        methods: {
            init() {
                this.logoShowType = this.openUserInfo.ext.logo.logoShowType;
                this.url = this.openUserInfo.ext.logo.url;
            },
            save() {
                this.openUserInfo.ext.logo.logoShowType = this.logoShowType;
                if (this.logoShowType == 'url' && this.url == '') {
                    this.$toast.warning('请选择一个图片上传');
                    return;
                }

                this.openUserInfo.ext.logo.url = this.url;
                this.$store.commit('uOpenUserInfo', this.openUserInfo);
                this.$toast.success('保存成功');
            },
        }
    }
</script>

<style scoped>

    .module {
        /*padding: 10px 10px 0;*/
        display: grid;
        grid-template-columns: 1fr;
        grid-row-gap: 20px;
    }

    .tab {
        display: grid;
        grid-template-columns: repeat(4, auto);
        justify-content: left;
        grid-row-gap: 10px;
        grid-column-gap: 20px;
    }

    .item {
        display: grid;
        grid-template-columns: 20px 1fr;
        place-items: center;
        place-content: center;
        cursor: pointer;

    }


    .row {
        height: 120px;
        text-align: center;

        display: grid;
        grid-template-columns: 1fr;
        place-content: center;
        place-items: center;
    }

    .img {

        height: 100px;
        width: 200px;

        border: 1px dashed #DCDFE6;;

        display: grid;
        place-content: center;
        place-items: center;
        cursor: pointer;


    }

    .img > img {
        height: 100px;
        width: 200px;
    }

    .img > i {
        font-size: 40px;
    }

    .time {
        height: 100px;
        width: 200px;

        border: 1px dashed #DCDFE6;;

        display: grid;
        place-content: center;
        place-items: center;
    }

    .time > span {
        font-size: 40px;
    }

    .weather {
        height: 100px;
        width: 200px;

        display: grid;
        place-content: center;
        place-items: center;
    }

    input {
        outline: none;
        border: none;
        width: 100%;
        background: transparent;
    }


    input[type=radio] {
        -webkit-appearance: none;
        border: 1px orange solid;
        width: 10px;
        height: 10px;
        border-radius: 50%;
    }


    input[type=radio]:focus, input[type=radio]:checked {
        -webkit-appearance: none;

        background-color: coral;
    }

    label {
        cursor: pointer;
    }


    .btn-group {
        width: 100%;
        margin-top: 10px;

        display: grid;
        grid-row-gap: 10px;
    }

    button {
        height: 40px;
    }
</style>