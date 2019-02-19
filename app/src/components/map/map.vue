<template>
    <div class="mb map" id="map">

    </div>
</template>

<script>
    import AMap from 'AMap'
    export default {
        data() {
            return {
                address:{},
            }
        },
        mounted(){
            var lnglatXY = [118.167222,24.460929]; //已知点坐标
            var geolocation;
            var arr = [];
            var map = new AMap.Map('map',{
                resizeEnable: true,
                zoom: 17,
                center: lnglatXY,
                //pitch:35,
                //viewMode:'3D',
            });
/*            //为地图注册click事件获取鼠标点击出的经纬度坐标
            var clickEventListener = map.on('click', function(e) {
                console.log( e.lnglat.getLng() , e.lnglat.getLat() )
            });
*/

            let self = this;
            let marker = new AMap.Marker({
                position: map.getCenter()
            });
            marker.setMap(map);

            function fn(){
                //console.log( self.address.address )
                // 设置鼠标划过点标记显示的文字提示
                marker.setTitle(self.address.address);

                // 设置label标签
                marker.setLabel({//label默认蓝框白底左上角显示，样式className为：amap-marker-label
                    offset: new AMap.Pixel(-55, -36),//修改label相对于maker的位置
                    content: self.address.address
                });
            }

            //解析定位结果
            let onComplete = (data) => {
                /*let str=['定位成功'];
                str.push('经度：' + data.position.getLng());
                str.push('纬度：' + data.position.getLat());
                str.push('详细信息：' + data.formattedAddress);

                arr.push(data.position.getLng(),data.position.getLat(),data.formattedAddress);
                if(data.accuracy){
                    str.push('精度：' + data.accuracy + ' 米');
                }//如为IP精确定位结果则没有精度信息
                str.push('是否经过偏移：' + (data.isConverted ? '是' : '否'));
                //document.title += str.join('<br>');
                //this.address = data.formattedAddress;
                //console.log(this,data.formattedAddress)*/
                this.$set(this.address,'address',data.formattedAddress); // 重要
                fn();

            }



            map.plugin('AMap.Geolocation', function() {
                geolocation = new AMap.Geolocation({
                    enableHighAccuracy: true,//是否使用高精度定位，默认:true
                    timeout: 10000,          //超过10秒后停止定位，默认：无穷大
                    //buttonOffset: new AMap.Pixel(10, 20),//定位按钮与设置的停靠位置的偏移量，默认：Pixel(10, 20)
                    zoomToAccuracy: true,      //定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
                    //buttonPosition:'RB'
                });
                //map.addControl(geolocation); // 不要圆点
                geolocation.getCurrentPosition();
                AMap.event.addListener(geolocation, 'complete', onComplete);//返回定位信息
                AMap.event.addListener(geolocation, 'error', onError);      //返回定位出错信息
            });


            //解析定位错误信息
            function onError(data) {
                //con.innerHTML += '定位失败';
            }

/*

            showCityInfo();
            //获取用户所在城市信息
            function showCityInfo() {
                //实例化城市查询类
                var citysearch = new AMap.CitySearch();
                //自动获取用户IP，返回当前城市
                citysearch.getLocalCity(function(status, result) {
                    if (status === 'complete' && result.info === 'OK') {
                        if (result && result.city && result.bounds) {
                            var cityinfo = result.city;
                            var citybounds = result.bounds;
                            //document.title= '您当前所在城市：'+'-'+cityinfo;
                            //地图显示当前城市
                            map.setBounds(citybounds);
                        }
                    } else {
                        console.log('错误信息+'+ result.info);
                    }
                });
            }
*/


        }
    }
</script>

<style scoped lang="less">
    @rem:750/10rem;
    .mb{
        margin-bottom: 130/@rem;
    }
    .map{
        height: 100%;
    }
    .amap-marker-label{
        padding: 10px 20px !important;
        background-color: rgba(255,255,255,0.2) !important;
    }
</style>
