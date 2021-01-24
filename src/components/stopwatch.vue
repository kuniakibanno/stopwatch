<template>
<div class="body">
    <div id="main">
        <div class="container1">
            <div class="title">５秒チャレンジ</div>
            <div　class="result">結果</div>
            <div id="timer">
                <transition name="time">
                    <div v-if="time" class="time">{{ time }}</div>
                </transition>
                <transition  name="register">
                    <div v-if="register">
                        <p class="register">結果をランキングに載せる</p>
                        <input type="text" v-model.trim="addName" v-on:keyup.enter="addRank()" placeholder="名前を入力">
                    </div>
                </transition>
            </div>
            <div class="controls">
                <div class="btn1" id="start" v-on:click="start()" v-bind:class="{inactive: isStart}" v-show="!active">Start</div>
                <div class="btn2" id="stop" v-on:click="stop()" v-bind:class="{inactive: isStop}" v-show="active">Stop</div>
                <div class="btn3" id="reset" v-on:click="reset()" v-bind:class="{inactive: isReset}">Reset</div>
            </div>
        </div>
        <div class="container2 ranks">
            <div class="ranking-title">--- ランキング ---</div>
            <ul class="rank">
                <li v-for="item in ranks">{{ item.name }}/{{ item.time }} ({{ item.difference }})</li>
            </ul>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: "stopwatch",
    data : function () {
        return {
        active : false,
        time: "",
        startTime: 0,
        erapsedTime: 0,
        isStart: false,
        isStop: true,
        isReset: true,
        timeoutId:0,
        register: false,
        addName:"",
        ranks:[
            {name:'しょう',time:'4.975',difference:'-0.025',absolute:'25'},
            {name:'ひろと',time:'5.111',difference:'+0.111',absolute:'111'},
            {name:'しょうた',time:'4.566',difference:'-0.434',absolute:'434'},
            {name:'つばさ',time:'5.497',difference:'+0.497',absolute:'497'},
            {name:'れん',time:'4.376',difference:'-0.624',absolute:'624'},
            {name:'だいき',time:'5.692',difference:'+0.692',absolute:'692'},
            {name:'けんた',time:'4.177',difference:'-0.823',absolute:'823'},
            {name:'そうた',time:'5.893',difference:'+0.893',absolute:'893'},
            {name:'たくや',time:'5.901',difference:'+0.901',absolute:'901'},
            {name:'だいき',time:'5.937',difference:'+0.937',absolute:'937'},
        ]
        }
    },

    methods: {
        countUp() {
            var d  = new Date(Date.now() - this.startTime + this.erapsedTime);
                this.timeoutId = setTimeout(() => {
                    this.countUp();
                }, 10);
        },

        setButtonStateInitial() {
            this.isStart = false;
            this.isStop = true;
            this.isReset = true;
        },

        setButtonStateRunning() {
            this.isStart = true;
            this.isStop = false;
            this.isReset = true;
        },

        setButtonStateStopped() {
            this.isStart = false;
            this.isStop = true;
            this.isReset = false;
        },

        start() {
            if (this.isStart) {
                return;
            }
            this.active = true;
            this.setButtonStateRunning();
            this.startTime = Date.now();
            this.countUp();
        },

        stop() {
            if (this.isStop) {
                return;
            }
            this.active = false;
            this.setButtonStateStopped();
            clearTimeout(this.timeoutId);
            this.erapsedTime += Date.now() - this.startTime;
            var d  = new Date(this.erapsedTime);
            var s  = String(d.getSeconds()).padStart(1, '0');
            var ms = String(d.getMilliseconds()).padStart(3, '0');
            this.time = `${s}.${ms}`;
            this.register = true;
        },

        reset() {
            if (this.isReset) {
                return;
            }
            this.time = '';
            this.setButtonStateInitial();
            this.erapsedTime = 0;
            this.register = false;
        },

        addRank(){
            if(this.addName){
                var difference = 5000 - this.erapsedTime;
                var absolute = Math.abs(difference);

                var flg = Math.sign(difference);
                if(flg >= 0){
                    var pm = "-";
                }else{
                    var pm = "+";
                }

                var d  = new Date(absolute);
                var s  = String(d.getSeconds()).padStart(1, '0');
                var ms = String(d.getMilliseconds()).padStart(3, '0');

                difference = `${pm}${s}.${ms}`;

                this.ranks.push({name:this.addName,time:this.time,difference:difference,absolute:absolute});
                this.addName = '';

                this.ranks.sort(function(a,b){
                    return a.absolute - b.absolute;
                });
            }
        }
    },
}
</script>

<style scoped>
.body {
    margin-top:0px;
    font-family: 'Alegreya Sans SC', Courier, monospace;
    font-size: 14px;
    background: #fff;
}

#main{
    margin-top:0px;
}

.title{
    font-family: 'sans-serif';
    font-size: 30px;
    margin-bottom:8px;
    letter-spacing: 12px;
}

.result{
    font-family: 'sans-serif';
    font-size: 20px;
}

.name{
    margin-top:5px;
}

.container1 {
    margin: 20px auto;
    width: 450px;
    background: rgb(246,246,246);
    padding: 15px;
    text-align: center;
}

.container2 {
    margin: 20px auto;
    width: 450px;
    background: rgb(246,246,246);
    padding: 15px;
    text-align: center;
    display: flex;
    align-items: center;
    flex-direction: column;
}

#timer {
    /* background: #ddd; */
    height: 200px;
}

.time{
    font-size: 85px;
    font-family: 'Courier New', Courier, monospace;
}

.register{
    font-family: 'Courier New', Courier, monospace;
    font-size: 16px;
    line-height: 0px;
    margin-top:40px;
}

input{
    font-size:16px;
}

.btn1 {
    width: 170px;
    height: 45px;
    line-height: 45px;
    background: #ddd;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    border-radius:30px;
    border:solid 1px;
    margin: 0 20px;
}

.btn2 {
    width: 170px;
    height: 45px;
    line-height: 45px;
    background: #ddd;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    border-radius:30px;
    border:solid 1px;
    margin: 0 20px;
}

.btn3 {
    width: 170px;
    height: 45px;
    line-height: 45px;
    background: #ddd;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    border-radius:30px;
    border:solid 1px;
    margin: 0 20px;
}

.controls {
    display: flex;
    justify-content: space-between;
    user-select: none;
}

.inactive {
    opacity: 0.6;
}

.time-enter {
    opacity: 0;
}

.time-enter-active {
    transition: opacity .5s;
}

.ranking-title{
    font-size:20px;
    font-weight: bold;
}

.rank{
    padding:0 5px 0 50px;
    text-align: center;
    font-size:22px;
}

ul{
    list-style-type:decimal;
}
</style>