<template>
    <div class="index ubye-index">
        <div class="l-index-top">
            <i @touchstart="fanhui()" class="l-top-dingwei-i"></i>
            <span class="l-top-biaoti">异常订单详情</span>
        </div>
        <div class="top-zhanwei"></div>
        <div v-if="dingdan" class="l-index-body">
            <div class="top">
                <span class="sp1">{{data.fromAreaName3}}</span>
                <span class="sp2"></span>
                <span class="sp1">{{data.toAreaName3}}</span>
            </div>
            <div class="huowuxinxi">
                <div class="toubu">
                    <span class="sp1"></span>
                    <span class="sp2">货物信息</span>
                </div>
                <div class="infos">
                    <span class="sp1">货物类型:</span>
                    <span class="sp2">{{data.goodsType}}</span>
                </div>
                <div class="infos">
                    <span class="sp1">总体积:</span>
                    <span class="sp2">{{data.cube}}立方米</span>
                </div>
                <div class="infos">
                    <span class="sp1">总重量:</span>
                    <span class="sp2">{{data.weight}}kg</span>
                </div>
                <!--<div class="infos">-->
                    <!--<span class="sp1">包装方式:</span>-->
                    <!--<span class="sp2">集装箱</span>-->
                <!--</div>-->
                <!--<div class="infos">-->
                    <!--<span class="sp1">装货时间:</span>-->
                    <!--<span class="sp2">2019-04-10</span>-->
                <!--</div>-->
                <!--<div class="infos">-->
                    <!--<span class="sp1">到货时间:</span>-->
                    <!--<span class="sp2">2019-04-20</span>-->
                <!--</div>-->
                <div class="infos">
                    <span class="sp1">收货人:</span>
                    <span class="sp2">{{data.receiveName}}</span>
                </div>
                <div class="infos">
                    <span class="sp1">收货人电话:</span>
                    <span class="sp2">{{data.receivePhone}}</span>
                </div>
                <div class="infos">
                    <span class="sp1">收货地址:</span>
                    <span class="sp2">{{data.getAddressInfo}}</span>
                </div>
                <div class="infos">
                    <span class="sp1">所需车辆类型:</span>
                    <span class="sp2">{{data.carType}}</span>
                </div>
                <div class="infos">
                    <span class="sp1">备注:</span>
                    <span class="sp2">{{data.remark}}</span>
                </div>
                <div v-if="imgs.length > 0" class="toubu tbbu1">
                    <span class="sp1"></span>
                    <span class="sp2">货物图片</span>
                </div>
            </div>
            <div class="imgs">
                <div v-for="(item, index) in imgs"
                     :style="[{'height': [imgH + 'px']},
                      {'width': [imgH + 'px']},
                      {'background': [`url(${item.src}) no-repeat center`]},
                      {'background-size': [item.w + item.h]},]"
                     class="sp1">
                </div>
                <span id="span0" class="sp1 sp2"></span>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                imgH: '',
                imgs: [],
                dingdan: true,
                data: {},
            }
        },

        mounted() {
            var th = this
            var w = (window.innerWidth - 60) / 3
            th.imgH = w
            this.getOne()
        },

        methods: {

            getOne() {
                var th = this
                var data = {
                    id: th.$route.query.id,
                    type: 'order',
                }
                th.ajax.get('/common/api/getOneData', {params: data})
                    .then((r) => {
                        if (r.resultCode === 0) {
                            th.data = r.data
                            var imgs = r.data.imgList
                            for (var i = 0; i < imgs.length; i++) {
                                var img = imgs[i]
                                th.newImg(img, i)
                            }
                        }
                    })
            },

            fanhui() {
                history.go(-1)
            },

            jieshu() {
                this.$router.push('/index')
            },

            newImg(src, i) {
                var th = this
                var img = new Image()
                var obj = {
                    h: '',
                    w: '',
                    src: src,
                }
                img.src = src
                img.onload = function() {
                    if (img.width > img.height) {
                        obj.h = '100%'
                        var bili = img.width / img.height
                        obj.w = (Math.round(bili * 10000)/100).toFixed(2) + '%'
                    } else if (img.width < img.height) {
                        var bili = img.height / img.width
                        obj.h = (Math.round(bili * 10000)/100).toFixed(2) + '%'
                        obj.w = '100%'
                    } else {
                        obj.h = '100%'
                        obj.w = '100%'
                    }
                    th.imgs.push(obj)
                }
            },

            runStart() {
                this.$router.push('/jiedanzhong')
            },
        }
    }
</script>

<style lang="scss" scoped>

    .ubye-index {
        height: 100vh;
        .kong {
            position: fixed;
            top: 0;
            width: 100%;
            height: 100vh;
            background: #f2f2f2;
            display: flex;
            flex-direction: column;
            align-items: center;
            img {
                width: 30%;
                margin-top: 40%;
                margin-bottom: 30px;
            }
            span {
                color: #999;
                font-size: 12px;
            }
        }
    }

    .l-index-dingdan {
        position: relative;
    }

    .l-index-dingdan .l-xiaohongdian {
        top: -7px;
    }

    .l-index-jiwdanshuoming {
        font-size: 14px;
        width: 100%;
        padding: 0 50px;
        box-sizing: border-box;
        color: #333;
        margin: 40px 0 10px 0;
    }

    .l-index-img-1 {
        width: 75px;
        height: 75px;
        margin-top: 25px;
    }

    .l-index-img-2 {
        width: 200px;
        height: 100px;
        margin-top: 40px;
    }

    .l-index-shuoming {
        width: 100%;
        padding: 0 50px;
        box-sizing: border-box;
    }

    .l-index-shuoming span {
        width: 100%;
        font-size: 14px;
        color: #333;
    }

    .l-index-shuoming div {
        font-size: 12px;
        color: #666;
        line-height: 20px;
    }

    .l-index-body {
        .buttons {
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 20px;
            box-sizing: border-box;
            margin: 15px 0 35px 0;
            .sp1 {
                border: solid 1px #51c1ea;
                color: #51c1ea;
            }
            .sp2 {
                color: #fff;
                background: #51c1ea;
            }
            span {
                width: 150px;
                height: 42px;
                border-radius: 5px;
                font-size: 16px;
                display: flex;
                justify-content: center;
                align-items: center;
            }
        }
        .imgs {
            padding-left: 15px;
            box-sizing: border-box;
            width: 100%;
            display: flex;
            flex-wrap: wrap;
            /*justify-content: space-around;*/
            .sp2 {
                position: absolute;
                left: 10066px;
                height: 0;
            }
            .sp1 {
                margin: 0 15px 15px 0;
                border: solid 1px #ddd;
                display: flex;
                justify-content: center;
                align-items: center;
                overflow: hidden;
                img {

                }
            }
        }
        width: 100%;
        display: flex;
        align-items: center;
        flex-direction: column;
        background: #fff;
        .huowuxinxi {
            .infos {
                width: 100%;
                display: flex;
                position: relative;
                padding-left: 95px;
                box-sizing: border-box;
                margin-bottom: 5px;
                .sp1 {
                    width: 95px;
                    color: #666;
                    font-size: 12px;
                    position: absolute;
                    left: 0;
                }
                .sp2 {
                    width: 100%;
                    color: #666;
                    font-size: 12px;
                }
            }
            .tbbu1 {
                margin-top: 10px;
            }
            .toubu {
                .sp1 {
                    width: 2px;
                    height: 16px;
                    background: #51c1ea;
                    position: relative;
                    top: 3px;
                    margin-right: 5px;
                }
                .sp2 {
                    color: #333;
                    font-size: 16px;
                }
                height: 30px;
                width: 100%;
                border-bottom: solid 1px #ddd;
                display: flex;
                margin-bottom: 15px;
            }
            width: 100%;
            padding: 0 15px;
            box-sizing: border-box;
            display: flex;
            flex-direction: column;
        }
        .top {
            width: 100%;
            height: 65px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            .sp1 {
                color: #51c1ea;
            }
            .sp2 {
                color: #fff;
                background: url("../../../static/images/dkdajmtb.png") no-repeat center;
                background-size: 100% 100%;
                border-radius: 50%;
                display: flex;
                justify-content: center;
                align-items: center;
                width: 25px;
                height: 25px;
                margin: 0 25px;
            }
        }
    }

    .l-index-body-button {
        width: 145px;
        height: 45px;
        background: #51c1ea;
        border-radius: 5px;
        border: none;
        color: #fff;
        font-size: 16px;
        margin-top: 60px;
    }

    .l-top-biaoti {
        width: 100%;
        top: 0;
        height: 50px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #fff;
        font-size: 0.36rem;
        padding: 0 50px 0 20px;
        box-sizing: border-box;
        position: absolute;
    }

    .l-xiaohongdian {
        min-width: 15px;
        max-width: 25px;
        height: 15px;
        border-radius: 50%;
        background: #f85156;
        color: #fff;
        font-size: 12px;
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        top: 7px;
        right: 18px;
        padding: 0 4px;
    }

    .l-top-info {
        width: 70px;
        height: 100%;
        display: flex;
        align-items: center;
        position: relative;
        .l-xiaohongdian {
            right: -7px;
        }
    }

    .top-zhanwei {
        width: 100%;
        height: 50px;
    }

    .l-top-dingwei-i {
        display: inline-block;
        width: 0.8rem;
        height: 0.8rem;
        background: url("../../../static/images/fanhuixiangyb.png") no-repeat left center;
        background-size: 0.2rem 0.36rem;
        position: relative;
        z-index: 103;
    }

    .l-index-top {
        width: 100%;
        height: 1rem;
        position: fixed;
        top: 0;
        padding: 0 15px;
        background: #51c1ea;
        display: flex;
        align-items: center;
        justify-content: space-between;
        box-sizing: border-box;
        z-index: 120;
    }

    .isActive {
        font-size: 10px;
        color: #51c1ea!important;
    }

    .l-jiehuo {
        color: #000;
        font-size: 12px;
        position: absolute;
        bottom: 6px;
        background: #fff;
        z-index: 102;
    }

    .l-zhongjian-div {
        position: absolute;
        width: 40px;
        height: 100%;
        display: flex;
        justify-content: center;
        top: 0;
        z-index: 105;
    }

    .l-zhongjian {
        width: 40px;
        height: 40px;
        background: #fff;
        border-radius: 50%;
        position: relative;
        top: -10px;
        display: flex;
        justify-content: center;
    }

    .l-zhongjian-zhongjian-vevk {
        width: 45px;
        height: 30px;
        background: #fff;
        position: absolute;
        z-index: 101;
    }

    .zhongjian-i {
        background: url("../../../static/images/fahuo1.png") no-repeat center;
        background-size: 100% 100%;
        display: inline-block;
        width: 30px;
        height: 30px;
    }

    .l-zhongjian-zhongjian {
        width: 30px;
        height: 30px;
        /*background: #51c1ea;*/
        border-radius: 50%;
        z-index: 102;
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        top: 3px;
    }

    .index{
        width: 100%;
        height: 100%;
        background: #fff;
    }



</style>
