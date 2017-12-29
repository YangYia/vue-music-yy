<style scoped lang='stylus'>
.player {
    width: 100%;
    background: #fff;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1;
    header {
        height: 50px;
        line-height: 50px;
        text-align: center;
        font-size: 12px;
        color: #333;
        position: relative;
        color: #fff;
        span {
            width: 50px;
            height: 50px;
            text-align: center;
            line-height: 50px;
            font-size: 24px;
            display: block;
            position: absolute;
            top: 0;
            left: 0;
        }
    }
    .content_cd {
        width: 100%;
        padding-top: 50px;
        position: relative;
        overflow: hidden;
        .cd {
            width: 100%;
            position: relative;
            animation: rotate 5s linear infinite paused;
            img.imgCd {
                width: 70%;
                margin: 0 auto;
                display: block;
                border-radius: 9999px;
                box-shadow: 0px 0px 30px #fff;
                border: 10px solid rgba(255, 255, 255, 0.6);
            }
            .singerImg {
                width: 40%;
                position: absolute;
                top: 50%;
                left: 50%;

                transform: translate(-50%, -50%);
                overflow: hidden;
                border-radius: 50%;
                img {
                    width: 100%;
                    display: block;
                }
            }
        }
        .cd.cur {
            animation-play-state: running;
        }
        @keyframes rotate {
            from {
                transform: rotate(0deg);
            }
            to {
                transform: rotate(360deg);
            }
        }
        .swith {
            width: 110px;
            height: 165px;
            position: absolute;
            top: -18px;
            left: 62%;
            transform: translateX(-50%) rotate(-23deg);
            /*这个属性 调整 rotate 圆心点*/
            transform-origin: 15px 15px;
            transition: 1s ease all 0s;
            z-index: 5;
            img {
                width: 100%;
            }
        }
        .swith.cur {
            transform: translateX(-50%) rotate(0deg);
        }
    }
    .setting {
        padding-top: 50px;
        ul {
            overflow: hidden;
            margin: 0 auto;
            width: 70%;
            li {
                float: left;
                width: 25%;
                height: 50px;
                text-align: center;
                line-height: 50px;
                font-size: 24px; color: #fff;
            }
        }
    }
    .timeBar {
        width: 100%;
        height: 40px;
        .startTime {
            width: 15%;
            height: 40px;
            line-height: 40px;
            font-size: 1px;
            text-align: right;
            color: rgba(255, 255, 255, 0.8);
        }
        .all_duration {
            width: 60%;
            height: 2px;
            margin: 19px 5%;
            span.duration {
                position: relative;
                height: 2px;
                width: 100%;
                background: red;
                display: block;
                span.currentTime {
                    position: absolute;
                    top: 0;
                    left: 0;
                    height: 2px;
                    width: 20%;
                    background: green;
                    display: block;
                }
            }
        }
        .endTime {
            width: 15%;
            height: 40px;
            line-height: 40px;
            font-size: 1px;
            text-align: left;
            color: rgba(255, 255, 255, 0.8);
        }
    }
    .controller {
        ul {
            overflow: hidden;
            li {
                float: left;
                width: 20%;
                height: 50px;
                text-align: center;
                line-height: 50px;
                font-size: 30px;
                color: rgba(255, 255, 255, 0.7);
            }
            li.icon-bofang,
            li.icon-pause-20 {
                font-size: 48px;
            }
        }
    }
    .popWindowAudioPlay {
        width: 100%;
        background: rgba(0, 0, 0, 0.7);
        z-index: -1;
        position: fixed;
        top: 0;
        left: 0;
        opacity: 0;
        transition: 1s ease all 0s;
        .audioPlayList {
            position: absolute;
            bottom: -240px;
            left: 0;
            width: 100%;
            height: 240px;
            overflow-y: scroll;
            background: #fff;
            transition: 1s ease all 0s;
            div {
                overflow: hidden;
                height: 40px;
                span {
                    float: right;
                    width: 40px;
                    height: 40px;
                    line-height: 40px;
                    text-align: center;
                    font-size: 24px;
                }
            }
            ul {
                li {
                    border-top: 1px solid #ccc;
                    overflow: hidden;
                    height: 40px;
                    line-height: 40px;
                    font-size: 14px;
                    text-indent: 1em; 
                    
                    span {
                        float: right;
                        width: 40px;
                        height: 40px;
                        line-height: 40px;
                        text-align: center;
                        font-size: 24px;
                        position: relative;
                        right: 10px;
                    }
                    p{
                        /*文本一行显示*/
                        overflow: hidden;
                        display: -webkit-box;
                        -webkit-line-clamp: 1;
                        -webkit-box-orient: vertical;
                        height: 40px;
                        line-height: 40px;
                        font-size: 14px;
                        text-indent: 1em;
                        width: 85%;float: left;
                    }
                }
                li.cur {
                    color: red;
                }
            }
        }
    }
    .popWindowAudioPlay.cur {
        opacity: 1;
        z-index: 2;
    }
    .popWindowAudioPlay.cur .audioPlayList {
        bottom: 0;
    }
}
.lyric{
    position: relative;overflow: hidden;
    ul{
        position: absolute;width: 100%;transition: 1s all ease 0s;
        li{
            text-align: center;font-size: 1px;line-height: 20px;color: #fff;min-height: 20px;
        }
        li.cur{
            color: red;
        }
    }
}
</style>
<template>
    <div class="player" v-height :style="{background :'#223950 url('+ player.playerBg +') 0 0 /100% 100% no-repeat'}">
        <span v-if="typeof player.index == 'number'">   
            <audio id="audio" :src="player.album[player.index].musicUrl" autoplay :loop="playState == 'once'" @timeupdate="play()"></audio> 
        </span>
        <header>
            {{player.index == null ? '播放器' : player.album[player.index].musicName}}
            <span @click="goBack" class="iconfont icon-houtui1"></span>
        </header>
        <div class="content_cd" v-show="!isShowLyric" @click="showLyric" v-height="300">
            <div class="swith" :class="{cur :addSwithClass}">
                <img src="../../resource/img/swith.png">
            </div>
            <div class="cd" :class="{cur :addSwithClass}">
                <img src="../../resource/img/cd.png" class="imgCd">
                <div class="singerImg">
                    <img :src="player.albumImgSrc" alt="">
                </div>
            </div>
        </div>
        <div class="lyric" v-show="isShowLyric" @click="hideLyric" v-height="250">
            <ul>
                <li v-for="item in lyric">{{item[1]}}</li>
            </ul>
        </div>
        <div class="setting">
            <ul>
                <li class="iconfont icon-xin1"></li>
                <li class="iconfont icon-xiazai1"></li>
                <li class="iconfont icon-pinglun"></li>
                <li class="iconfont icon-more-vert"></li>
            </ul>
        </div>
        <div class="timeBar ovh">
            <p class="startTime fl">{{currentTime | zhuanhuan}}</p>
            <p class="all_duration fl">
                <span class="duration" @click="goTime($event)">
                    <span class="currentTime" :style="{width:currentTime/duration*100+'%'}"></span>
                </span>
            </p>
            <p class="endTime fl">{{duration | zhuanhuan}}</p>
        </div>
        <div class="controller">
            <ul>
                <li class="iconfont icon-renwu_xh"></li>
                <li class="iconfont icon-previous" @click="goprev"></li>
                <!-- :class="[{'icon-bofang' : addSwithClass},{'icon-pause-20' : !addSwithClass}]" -->
                <li class="iconfont " @click="play_pause" :class="[!addSwithClass == true ? 'icon-bofang' : 'icon-pause-20']"></li>
                <li class="iconfont icon-next" @click="gonext"></li>
                <li class="iconfont icon-icon8" @click="showPopWindowAudioPlay"></li>
            </ul>
        </div>
        <div class="popWindowAudioPlay" v-height :class="{cur:isShowPopWindowAudioPlay}">
            <div class="audioPlayList">
                <div>
                    <span class="iconfont icon-guanbi" @click="close"></span>
                </div>
                <ul>
                    <li v-for="(item, index) in player.album" :class="{cur : index == player.index}" @click="audioPlayListIndex(index)">
                        <p>
                            {{item.musicSinger}} - {{item.musicName}}
                        </p>
                        <span class="iconfont icon-bofang"></span>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: ["player"],
    data() {
        return {
            currentTime: 0,
            duration: 0,
            // 添加旋转 和 磁头
            addSwithClass: false,
            isAddSwithClass: true,
            isShowPopWindowAudioPlay: false,
            isNext: true,
            isShowLyric: false,
            lyric:[]
        }
    },
    computed: {
        index() {
            return this.player.index
        },
        playState(){
            return this.$store.state.playState
        }
    },
    watch:{
        index(){
            var vm = this; 
            if(this.player.album[this.index].lyric){
                $.get(this.player.album[this.index].lyric, function(data) { 
                    vm.lyric = vm.parseLyric(data)
                })
            }
            
        }
    },
    filters: {
        zhuanhuan: function(s) {
            var t;
            if (s > -1) {
                var min = Math.floor(s / 60) % 60;
                var sec = s % 60;
                if (min < 10) { t += "0"; }
                t = min + ":";
                if (sec < 10) { t += "0"; }
                t += ~~sec;
            }
            return t;
        }
    },
    methods: {
        hideLyric() {
            this.isShowLyric = false
        },
        showLyric() {
            var vm = this;
            this.isShowLyric = true
            if(this.player.album[this.index].lyric){
                $.get(this.player.album[this.index].lyric, function(data) { 
                    vm.lyric = vm.parseLyric(data)
                })
            }
        },
        parseLyric(lyric){
            // console.log(lyric);
            var lines = lyric.split(/\n/); //去掉换行
            var getLyricTime = /\[\d{2}:\d{2}.\d{2}\]/g //匹配时间
            var arr = []
            while(!getLyricTime.test(lines[0])){
                lines = lines.splice(1)
                // 得到 有时间的歌词
            }
            if(lines[lines.length - 1].length === 0){
                lines.pop()
            }
            lines.forEach(function(item){
                var index = item.indexOf(']');
                var time = item.substr(1,index - 1)
                var timeArr = time.split(":")
                var geci = item.substr(index + 1)
                arr.push([timeArr[0] * 60 + parseFloat(timeArr[1]),geci])
            })
            arr.sort(function(a,b){
                return a[0] - b[0]
            })
            return arr 
        },
        goprev() {
            // 播放上一曲
            this.addSwithClass = true
            if (this.index == 0) {
                this.playIndex(0)
            } else {
                this.playIndex(this.index - 1)
            }
        },
        gonext() {
            // 播放下一曲
            this.addSwithClass = true
            if (this.index >= this.player.album.length - 1) {
                this.playIndex(this.player.album.length - 1)
            } else {
                this.playIndex(this.index + 1)
            }
        },
        goTime(event) {
            // 点击进度条 实现快进快退 
            // 方式 就是点击的位置 / 进度条的总宽 * 当前歌曲的总时间
            $("#audio")[0].currentTime = event.offsetX / $(".duration").width() * this.duration
        },
        playIndex(index) {
            this.addSwithClass = true
            if (this.$store.state.player.playType == 0) {
                this.$store.commit("PLAYAUDIO", {
                    playType: 0,
                    isShowPlayer: true,
                    album: this.player.album,
                    index: index,
                    playerBg: this.player.album[index].playerBg,
                    albumImgSrc: this.player.album[index].albumImgSrc
                })
            } else if (this.$store.state.player.playType == 1) {
                this.$store.commit("PLAYAUDIO", {
                    playType: 1,
                    isShowPlayer: true,
                    album: this.player.album,
                    index: index,
                    playerBg: this.player.playerBg,
                    albumImgSrc: this.player.albumImgSrc
                })
            }
        },
        audioPlayListIndex(index) {
            // 播放器的里面的列表点击事件 使用 0 和 1对 播放专辑 和 热门歌曲做区分
            this.playIndex(index)
        },
        close() {
            // 关闭 播放器里面的 列表
            this.isShowPopWindowAudioPlay = false
        },
        showPopWindowAudioPlay() {
            // 点击显示播放器的 里面的歌曲列表
            // 使用的是 透明度  和z-index 为负数 加类的时候 显示 并将z-index为正数
            this.isShowPopWindowAudioPlay = true
        },
        play_pause() {
            // 点击播放暂停
            var paused = document.getElementById("audio").paused
            if (!paused) {
                document.getElementById("audio").pause();
                this.isAddSwithClass = false
                this.addSwithClass = false
            } else {
                document.getElementById("audio").play();
                this.isAddSwithClass = true
                this.addSwithClass = true
            }
        },
        goBack() {
            this.$store.commit("SHOWORHIDEPLAYER", {
                isShowPlayer: false
            })
        },
        play() {
            var vm = this;
            var currentTime = document.getElementById("audio").currentTime
            var duration = document.getElementById("audio").duration
            var paused = document.getElementById("audio").paused
            var ended = document.getElementById("audio").ended
            // console.log(ended);
            if (ended && this.isNext) {
                this.isNext = false;
                if (this.index == this.player.album.length - 1 && ended) {
                    document.getElementById("audio").pause()
                    this.addSwithClass = false

                } else {
                    this.gonext();
                }
                this.isNext = true;
            }
            // 判断是不是暂停状态
            if (!paused) {
                if (this.isAddSwithClass) {
                    this.isAddSwithClass = false
                    this.addSwithClass = true
                }
            }
            vm.currentTime = currentTime;
            vm.duration = duration;  
            for(var p = 0; p < vm.lyric.length - 1; p++){
                if(vm.currentTime > vm.lyric[p][0]){
                    // console.log(vm.lyric[p][0]);
                    $(".lyric ul li").removeClass('cur')
                    $(".lyric ul li").eq(p).addClass('cur')
                    $(".lyric ul").css("top", $(".lyric").height() / 2 -  20 * ( p + 1))
                }
            }
        }
    }
}

</script>
