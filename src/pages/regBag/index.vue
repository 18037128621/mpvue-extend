<template>
  <map id="myMap" 
  :longitude="longitude" 
  :latitude="latitude" 
  :scale="scale" 
  :markers="markers" 
  :circles="circles"
  @markertap="markertap"
  show-location>
    <cover-image class='redtask' src='http://oyiqi2hq0.bkt.clouddn.com/redtask.png' @click='goredtask'></cover-image>
    <!--我的红包-->
    <cover-image class='myred' src='http://oyiqi2hq0.bkt.clouddn.com/myred.png' @click='gomyredbag'></cover-image>
    <cover-image class='Refresh' src='/static/img/ref.png'  @click="moveToLocation"></cover-image>
    <cover-view class='btnRed' @click='nivetoIndex'>发红包</cover-view>

    <cover-view class='Notice'>
        <cover-image src='/static/img/mess.png'></cover-image>
        <cover-view class="span">恭喜您抢到一个红包，请注意查收!</cover-view>
    </cover-view>

      <!--开红包-->
    <cover-view class='bg-opa-black' v-bind:class="[markerId ? 'active' : '']">
        <cover-view class='red-open'>
        <cover-image src='/static/img/bg.png'></cover-image>
        <cover-view  class='red_logo'>
            <cover-image :src='redBag.logo'></cover-image>
        </cover-view>
        <cover-view class='red_tit'>
            <cover-view class='zan-col zan-col-18 zan-col-offset-3'>{{redBag.title}}</cover-view>
        </cover-view>
        <cover-view class='btn-open' bindtap='openRed'></cover-view>
        </cover-view>
    </cover-view>
  </map>
</template>

<script>
    //1.【组件】--引入组件
  import RedDilog from '../../components/red-dilog';
    //1.【地图】--引入qq地图对象
  const QQMapWX = require('../../utils/map/qqmap-wx-jssdk.js');
  import Fly from "flyio/dist/npm/wx";
  let fly = new Fly; //创建fly实例
     export default {
         //2.【组件】--组件实例化
         components: {
            RedDilog,
        },
         data(){
             return{
                scale: 16,
                city: "",
                latitude: "28",
                longitude: "113",
                markers: [],
                circles: [],
                redBag:{
                    logo:"http://oyiqi2hq0.bkt.clouddn.com/logo.png",
                    title:"湖南爱买单"
                },
                markerId:null
             }
         },
         methods:{
             //点击红包获取红包信息
             markertap:function(e){
                this.markerId=e.mp.markerId
                console.log(this.markerId)
                var redBag={}
                for(let i=this.markers.length;i--;){
                    if(this.markerId==this.markers[i].id){
                        console.log(this.markers[i])
                        redBag.logo=this.markers[i].iconUrl
                        redBag.title=this.markers[i].title
                    }
                }
                this.redBag=redBag
             }
         },
         mounted(){
             let _this=this;
             // 2.【地图】--实例化API核心类
            var qqmapsdk = new QQMapWX({
            key: 'XCEBZ-MEE3F-XAZJN-NKBX7-HXLTS-BIF6J'
            })
            //1.获取当前坐标
             wx.getLocation({
                 type:"gcj02",
                 success:function(res){
                     let {latitude,longitude} = res
                    _this.latitude=latitude
                    _this.longitude=longitude
                    _this.circles=[{
                        latitude: latitude,
                        longitude: longitude,
                        color: '#FF0000DD',
                        fillColor: '#7cb5ec88',
                        radius: 1500,
                        strokeWidth: 1
                    }]
                    console.log(res)
                    console.log(_this.longitude)
                    //2.请求完成坐标之后通过腾讯接口确认城市
                    qqmapsdk.reverseGeocoder({
                        location: {
                            latitude: _this.latitude,
                            longitude: _this.longitude
                        },
                        fail: function (res) {
                            console.log(res)
                        },
                        //3.凑齐城市，当前位置经纬度，请求红包信息
                        complete: function (res) {
                            console.log(res)
                            console.log(res.result.address)
                            _this.city = res.result.address_component.city
                            fly.post("https://wechat.umsamd.com/Amaidan-Market-Web/market/salRedEnvelopeInfo/selectStoreAndRedEnvelopeByRangeForWXProgram",{
                                city: _this.city,
                                latitude: _this.latitude,
                                longitude: _this.longitude,
                                loginUserId: "13",
                                gender: 0,
                                iconPath: "/static/img/my.png",
                                width: 40,
                                height: 40
                            }).then(res => {
                                
                                _this.markers=res.data.data
                                console.log(_this.markers);
                            })
                        }
                    })
                 }
             })
             
         },
     }
</script>

<style>
  #myMap {
    width: 100%;
    height: 100%;
    position:relative;
  }
  .btnRed{
  width: 50%;
  height:75rpx;
  line-height: 75rpx;
  border-radius: 5rpx;
  position: absolute;
  bottom: 70rpx;
  text-align: center;
  margin-left: -25%;
  left: 50%;
  color: #fff;
  font-size: 24rpx;
  color:#FFFFE3;
  background: #D75243;
  background-size: cover;
}
.Refresh{
  width: 50rpx;
  height: 50rpx;
  position: absolute;
  left: 40rpx;
  bottom: 260rpx;
  cursor:pointer;
}
/*通知*/
.Notice{
  position: absolute;
  top: 20rpx;
  left: 30rpx;
  width: 92%;
  height: 80rpx;
  line-height: 80rpx;
  color: #A5A5A5;
  font-size: 24rpx;
  background:#fff;
  display:flex;
  align-items: center;
  text-align: center;
}
.Notice .span{
    height: 60rpx;
    padding:10rpx;
    line-height:60rpx;
}
.Notice cover-image{
  width: 60rpx;
  height: 60rpx;
  padding:10rpx 20rpx;
}
/*蒙版*/
.bg-opa-black{
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0,0,0,0.5);
  display: none;
}
#myMap .active{
  display: block;
}
/*红包*/
#myMap .red-open{
  background: none;
  opacity:1;
  height:780rpx;
  position: absolute;
  top:45%;
  left:50%;
  width:520rpx;
  font-size:16px;
  overflow:hidden;
  border-radius:4px;
  transform:translate3d(-50%,-50%,0);
  color:#333;
  z-index:1;

}
.red-bg{}
.red_logo{
  width: 100%;
  position: absolute;
  top: 80rpx;
  text-align: center;
}
.red_logo cover-image{
  width:100rpx;
  height: 100rpx;
  margin: 0 auto;
  border-radius: 10rpx;
}
.red_tit{
  width: 100%;
  position: absolute;
  top: 210rpx;
  text-align: center;
  color: #Fcb79E;
}
.red_tit .zan-col,.red_text .zan-col{
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.red_text{
  width: 100%;
  position: absolute;
  top: 340rpx;
  text-align: center;
  color: #F5E9BA;
  font-size: 32rpx;
}
.btn-open{
  width: 100%;
  height: 400rpx;
  position: absolute;
  bottom: 0;
  cursor: pointer;
}


.myred{
  width: 84rpx;
  height: 84rpx;
  position: absolute;
  bottom: 150rpx;
  right: 30rpx;
}
.redtask{
  width: 84rpx;
  height: 84rpx;
  position: absolute;
  bottom: 240rpx;
  right: 30rpx;
}

/*导航*/
.nav{
  width: 100%;
  height: 82rpx;
  background: #FFF;
  position: absolute;
  left: 0;
  top: 0;
  display: flex;
}
.nav .zan-row{
  width: 100%;
  height: 82rpx;
}
.zan-row .zan-col{
  height: 82rpx;
  line-height: 82rpx;
  text-align: center;
  align-items: center;
  font-size: 28rpx;
  color: #999;
}
.zan-row .on{
  color: #f4374f;
}
</style>