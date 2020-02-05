<template>
    <view>
        <view id="just-tooltip"
              class="just-tooltip animated flipInBottom"
              :style="{width: 'auto', left: left+'px',top: top+'px',backgroundColor:backgroundColor,color:textColor}">
            <view class="just-con">
                <text class="item"
                      :style="{borderColor:splitColor}"
                      v-for="item in btns" @click="btnClick(item)">{{ item }}</text>
            </view>
            <view :class="justClass" :style="justStyleObject"></view>
        </view>

    </view>
</template>

<script>
    export default {
        name: 'BianmarenTooltip',
        props: {
            //是否显示
            tooltipShow:{
                type: Boolean,
                default:false
            },
            //颜色
            textColor:{
                type: String,
                default: "#ffffff"
            },
            //背景颜色
            backgroundColor:{
                type: String,
                default: "#000000"
            },
            //分隔符颜色
            splitColor:{
                type: String,
                default: "#ffffff"
            },
            //按钮组
            btns: {
                type: Array,
                default: ['HelloWorld',"你好啊"]
            },
            //点击元素Id
            eleId: {
                type: String,
                default: ""
            },
            //方向 bottom,top,left,right
            gravity:{
                type:String,
                default:"bottom"
            },
            distance:{
                type:Number,
                default:10
            }
        },
        data() {
            return {
                left:-9999,
                top:0,
                justStyleObject:{}
            };
        },
        watch:{
            eleId(){
                this.setPosition();
            },
            tooltipShow(){

                this.setPosition();
            }
        },
        mounted() {
            this.setPosition();
        },
        computed:{
            justClass(){
                return "just-"+this.gravity;
            }
        },
        methods:{
            setPosition(){

                var _this = this;

                if(!_this.tooltipShow){
                    _this.left = -9999;
                    return;
                }

                console.info(_this.parentThis)
                console.info(_this.eleId)

                const query = uni.createSelectorQuery().in(_this.$parent);
                query.select('#'+_this.eleId).boundingClientRect(data => {

                    var pos = { x: data.left, y: data.top };
                    var wh = { w: data.width, h: data.height };

                    console.info(pos)
                    console.info(wh)

                    var outerWidth = 0;
                    var outerHeight = 0;

                    const query2 = uni.createSelectorQuery().in(_this);

                    query2.select('#just-tooltip').boundingClientRect(dd => {

                        console.log(dd)

                        outerWidth = dd.width;
                        outerHeight = dd.height;

                        console.log("outerWidth:"+outerWidth);
                        console.log("outerHeight:"+outerHeight);

                        const systemInfo = uni.getSystemInfoSync();
                        var windowWidth = systemInfo.windowWidth;

                        var rightTmp = ( pos.x + wh.w / 2 ) + outerWidth / 2 ;
                        var leftTmp = ( pos.x + wh.w / 2 ) - outerWidth / 2 ;

                        if("top" === _this.gravity){
                            if(rightTmp > windowWidth ){
                                _this.left = pos.x + wh.w - outerWidth;
                                _this.top =  pos.y - outerHeight - _this.distance;
                                _this.justStyleObject = {
                                    'left':(outerWidth - wh.w/2) + "px",
                                    'borderColor':_this.backgroundColor+" transparent transparent transparent"
                                };
                            }else if( leftTmp < 0 ){
                                _this.left = pos.x;
                                _this.top = pos.y - outerHeight - _this.distance;
                                _this.justStyleObject = {
                                    'left':wh.w/2 + "px",
                                    'borderColor':_this.backgroundColor+" transparent transparent transparent"
                                };
                            }else{
                                _this.left = pos.x - (outerWidth - wh.w)/2;
                                _this.top = pos.y - outerHeight - _this.distance
                                _this.justStyleObject = {
                                    'left':"50%",
                                    'borderColor':_this.backgroundColor+" transparent transparent transparent"
                                };
                            }
                        }

                        if('bottom' === _this.gravity){
                            if(rightTmp > windowWidth ){
                                _this.left = pos.x + wh.w - outerWidth;
                                _this.top =  pos.y + wh.h + _this.distance;
                                _this.justStyleObject = {
                                    'left':(outerWidth - wh.w/2)+'px',
                                    'borderColor':"transparent transparent "+_this.backgroundColor+" transparent"
                                };
                            }else if( leftTmp < 0 ){
                                _this.left = pos.x;
                                _this.top =  pos.y + wh.h + _this.distance;
                                _this.justStyleObject = {
                                    left:wh.w/2 + 'px',
                                    'borderColor':"transparent transparent "+_this.backgroundColor+" transparent"
                                }
                            }else{
                                _this.left = pos.x - (outerWidth - wh.w)/2;
                                _this.top =  pos.y + wh.h + _this.distance;
                                _this.justStyleObject = {
                                    left:'50%',
                                    'borderColor':"transparent transparent "+_this.backgroundColor+" transparent"
                                }

                            }
                        }

                        if('left' === _this.gravity){
                            _this.left = pos.x - outerWidth - _this.distance;
                            _this.top = pos.y - (outerHeight - wh.h)/2;
                            _this.justStyleObject = {
                                top:'50%',
                                'borderColor':"transparent transparent transparent " +  _this.backgroundColor
                            }
                        }

                        if('right' === _this.gravity){
                            _this.left = pos.x + wh.w + _this.distance;
                            _this.top = pos.y - (outerHeight - wh.h)/2;
                            _this.justStyleObject = {
                                top:'50%',
                                'borderColor':"transparent "+_this.backgroundColor+" transparent  transparent"
                            }
                        }

                    }).exec();

                }).exec();

            },
            //按钮点击
            btnClick(btnName){
                this.$emit('btnClick',btnName)
            }
        }
    }
</script>

<style>

    /* just-Tips */
    .just-tooltip{position:absolute;left:0;top:0;border-radius:5px;background:#000;/*display:inline-block;*/z-index:9999;}
    .just-tooltip .just-con{padding:8px 10px;}

    .just-tooltip .just-top,
    .just-tooltip .just-bottom,
    .just-tooltip .just-left,
    .just-tooltip .just-right{content:"";position:absolute;width:0;height:0;overflow:hidden;border-style:solid;}

    .just-tooltip .just-top{left:50%;top:100%;border-width: 7px 5px 0 5px;margin-left:-5px;
        border-color: #1B1E24 transparent transparent transparent;
        _border-color: #1B1E24 #000000 #000000 #000000;
        _filter: progid:DXImageTransform.Microsoft.Chroma(color='#000000');
    }
    .just-tooltip .just-bottom{ left:50%; top:-7px;border-width: 0 5px 7px 5px;margin-left:-5px;
        border-color: transparent transparent #1B1E24 transparent;
        _border-color:#000000 #000000 #1B1E24 #000000;
        _filter: progid:DXImageTransform.Microsoft.Chroma(color='#000000');}
    .just-tooltip .just-left{ right:-7px; top:50%;border-width: 5px 0 5px 7px;margin-top:-5px;;
        border-color: transparent transparent transparent #1B1E24;
        _border-color:#000000 #000000 #000000 #1B1E24;
        _filter: progid:DXImageTransform.Microsoft.Chroma(color='#000000');
    }
    .just-tooltip .just-right{ left:-7px; top:50%;border-width: 5px 7px 5px 0;margin-top:-5px;
        border-color: transparent #1B1E24 transparent transparent;
        _border-color:#000000 #000000 #000000 #1B1E24;
        _filter: progid:DXImageTransform.Microsoft.Chroma(color='#000000');
    }

    .just-tooltip .just-confirm{text-align:center;padding:10px 0;margin:0 10px 10px 10px;}
    .just-tooltip .just-yes, .just-tooltip .just-no{background:#fff;color:#000;border:0;padding:5px 10px;}
    .just-tooltip .just-no{margin-left:10px;}

    /* Animations */
    .animated{
        -webkit-animation-fill-mode:both;-moz-animation-fill-mode:both;-ms-animation-fill-mode:both;-o-animation-fill-mode:both;animation-fill-mode:both;
        -webkit-animation-duration:.5s;-moz-animation-duration:.5s;-ms-animation-duration:.5s;-o-animation-duration:.5s;animation-duration:.5s;
    }
    @-webkit-keyframes flipInUp {
        0% { -webkit-transform: perspective(400px) rotateX(-90deg); opacity: 0;}
        40% { -webkit-transform: perspective(400px) rotateX(5deg);}
        70% { -webkit-transform: perspective(400px) rotateX(-5deg);}
        100% { -webkit-transform: perspective(400px) rotateX(0deg); opacity: 1;}
    }
    @-moz-keyframes flipInUp {
        0% {-moz-transform: perspective(400px) rotateX(-90deg);opacity: 0;}
        40% {-moz-transform: perspective(400px) rotateX(5deg);}
        70% {-moz-transform: perspective(400px) rotateX(-5deg);}
        100% {-moz-transform: perspective(400px) rotateX(0deg);opacity: 1;}
    }
    @-o-keyframes flipInUp {
        0% {-o-transform: perspective(400px) rotateX(-90deg);opacity: 0;}
        40% {-o-transform: perspective(400px) rotateX(5deg);}
        70% {-o-transform: perspective(400px) rotateX(-5deg);}
        100% {-o-transform: perspective(400px) rotateX(0deg);opacity: 1;}
    }
    @keyframes flipInUp {
        0% {transform: perspective(400px) rotateX(-90deg);opacity: 0;}
        40% {transform: perspective(400px) rotateX(5deg);}
        70% {transform: perspective(400px) rotateX(-5deg);}
        100% {transform: perspective(400px) rotateX(0deg);opacity: 1;}
    }
    @-webkit-keyframes flipInRight {
        0% { -webkit-transform: perspective(400px) rotateY(-90deg); opacity: 0;}
        40% { -webkit-transform: perspective(400px) rotateY(5deg);}
        70% { -webkit-transform: perspective(400px) rotateY(-5deg);}
        100% { -webkit-transform: perspective(400px) rotateY(0deg); opacity: 1;}
    }
    @-moz-keyframes flipInRight {
        0% {-moz-transform: perspective(400px) rotateY(-90deg);opacity: 0;}
        40% {-moz-transform: perspective(400px) rotateY(5deg);}
        70% {-moz-transform: perspective(400px) rotateY(-5deg);}
        100% {-moz-transform: perspective(400px) rotateY(0deg);opacity: 1;}
    }
    @-o-keyframes flipInRight {
        0% {-o-transform: perspective(400px) rotateY(-90deg);opacity: 0;}
        40% {-o-transform: perspective(400px) rotateY(5deg);}
        70% {-o-transform: perspective(400px) rotateY(-5deg);}
        100% {-o-transform: perspective(400px) rotateY(0deg);opacity: 1;}
    }
    @keyframes flipInRight {
        0% {transform: perspective(400px) rotateY(-90deg);opacity: 0;}
        40% {transform: perspective(400px) rotateY(5deg);}
        70% {transform: perspective(400px) rotateY(-5deg);}
        100% {transform: perspective(400px) rotateY(0deg);opacity: 1;}
    }
    .flipInTop, .flipInBottom .flipInLeft .flipInRight { -webkit-backface-visibility: visible !important; -moz-backface-visibility: visible !important; -o-backface-visibility: visible !important; backface-visibility: visible !important}
    .flipInTop, .flipInBottom { -webkit-animation-name: flipInUp; -moz-animation-name: flipInUp; -o-animation-name: flipInUp; animation-name: flipInUp; }
    .flipInLeft, .flipInRight { -webkit-animation-name: flipInRight; -moz-animation-name: flipInRight; -o-animation-name: flipInRight; animation-name: flipInRight; }

    @-webkit-keyframes fadeIn { 0% {opacity: 0;} 100% {opacity: 1;}}
    @-moz-keyframes fadeIn { 0% {opacity: 0;} 100% {opacity: 1;}}
    @-o-keyframes fadeIn {0% {opacity: 0;}100% {opacity: 1;}}
    @keyframes fadeIn {0% {opacity: 0;}100% {opacity: 1;}}

    .fadeIn{-webkit-animation-name: fadeIn; -moz-animation-name: fadeIn; -o-animation-name: fadeIn; animation-name: fadeIn;}

    .moveTop{
        -webkit-animation: moveTop .6s ease both;
        animation: moveTop .6s ease both;
    }
    .moveBottom{
        -webkit-animation: moveBottom .6s ease both;
        animation: moveBottom .6s ease both;
    }
    .moveLeft{
        -webkit-animation: moveLeft .6s ease both;
        animation: moveLeft .6s ease both;
    }
    .moveRight{
        -webkit-animation: moveRight .6s ease both;
        animation: moveRight .6s ease both;
    }
    @-webkit-keyframes moveTop {
        from {opacity: 0;-webkit-transform: translateY(-20px);}
        to {opacity: 1;-webkit-transform: translateY(0); }
    }
    @-webkit-keyframes moveBottom {
        from {opacity: 0;-webkit-transform: translateY(20px);}
        to {opacity: 1;-webkit-transform: translateY(0); }
    }
    @-webkit-keyframes moveLeft {
        from {opacity: 0;-webkit-transform: translateX(-20px);}
        to {opacity: 1;-webkit-transform: translateX(0); }
    }
    @-webkit-keyframes moveRight {
        from {opacity: 0;-webkit-transform: translateX(20px);}
        to {opacity: 1;-webkit-transform: translateX(0); }
    }

    .item{
        display: inline-block;
        border-right: #fff solid 1px;
        padding: 0 20rpx;
    }
    .item:last-child{
        border: none;
    }
</style>
