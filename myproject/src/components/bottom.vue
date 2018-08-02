<template>
    <div id="bottom" v-focus="true" @keydown="keyDown($event)" tabindex="-1">
        <div class="row">
            <button @click="display1('AC')" class="operator1">AC</button>
            <button @click="display1('+/-')" class="operator1">+/−</button>
            <button @click="display1('%')" class="operator1">%</button>
            <button @click="display2('÷')" class="operator2">÷</button>
        </div>
        <div class="row">
            <button @click="display4('7')" class="operator1">7</button>
            <button @click="display4('8')" class="operator1">8</button>
            <button @click="display4('9')" class="operator1">9</button>
            <button @click="display2('×')" class="operator2">×</button>
        </div>
        <div class="row">
            <button @click="display4('4')" class="operator1">4</button>
            <button @click="display4('5')" class="operator1">5</button>
            <button @click="display4('6')" class="operator1">6</button>
            <button @click="display2('-')" class="operator2">−</button>
        </div>
        <div class="row">
            <button @click="display4('1')" class="operator1">1</button>
            <button @click="display4('2')" class="operator1">2</button>
            <button @click="display4('3')" class="operator1">3</button>
            <button @click="display2('+')" class="operator2">+</button>
        </div>
        <div class="row" style="border-bottom-left-radius: 10px; border-bottom-right-radius: 10px;">
            <button @click="display4('0')" class="operator1" style="width: 271px;border-bottom-left-radius: 10px;">0</button>
            <button @click="display4('.')" class="operator1">.</button>
            <button @click="display3()" class="operator2" style="border-bottom-right-radius: 10px;">=</button>
        </div>
    </div>
</template>

<script>
export default {
    name:'Bottom',
    components: {

    },
    props: ['res','info'],
    data() {
        return {
        }
    },
    directives: {
        focus: {
            // 指令的定义
            inserted: function(el, binding) {
                if (binding.value) 
                    el.focus();
                else 
                    el.blur();
            },
            componentUpdated: function(el, binding) {
                if (binding.value)
                    el.focus();
                else 
                    el.blur();
          }
        }    
    },
    methods: {
       display1: function(str) {
           let res1 = this.res;
           let info1 = this.info;
            if(str == 'AC') {
                res1 = '';
                info1 = '';
            }

            if(str == '+/-') {
                if(info1 == '' && res1 != ''){
                    if(this.judgeNum(res1)){
                        if(res1[0] == '-') {
                            res1 = res1.substring(1,info1.length);
                        }
                        else if(res1[0] != '-') {
                            res1 = '-' + res1;
                        }
                    }
                }
                else if(info1[0] == '-') {
                    info1 = info1.substring(1,info1.length);
                }
                else if(info1[0] != '-') {
                    info1 = '-' + info1;
                }
            }

            if(str == '%') {
                if(info1 == '' && res1 == ''){
                    res1 = "invalid input!";
                }
                else if(info1 == '' && res1 != '') {
                    if(this.judgeNum(res1)){
                        res1 = String(parseFloat(res1)/100);

                    }
                }
                else if(this.judgeNum(info1)){
                    info1 = String(parseFloat(info1)/100);
                }
                else if(!this.judgeNum(info1)) {
                    res1 = "invalid input!";
                    info1 = '';
                }
            }
            this.$emit('getResult',res1);
            this.$emit('getInformation',info1); 
            return;
        },
        //加减
        display2: function(str) {
            let res1 = this.res;
            let info1 = this.info;
            if(res1 == '' && info1 == '') {
            }
            else if(res1 == '' && info1 != '') {
                let r1 = info1;
                if(this.judgeNum(r1))
                {
                    res1 = r1 + str;
                    info1 = '';
                }
                else{
                    res1 = "invalid input!";
                    info1 = '';
                }
            }
            else if(res1 != '' && info1 == '') { 
                if(this.judgeNum(res1)){
                    res1 = res1 + str;
                    info1 = '';
                }
                else{
                    res1 = 'invalid input!';
                }
            }
            else if(res1 != '' && info1 != '') {
                if(this.judgeNum(info1))
                {
                    this.calcu();
                    res1 = res1 + str;
                }
                else {
                    res1 = "invalid input!";
                    info1 = '';
                }
            }
            this.$emit('getResult',res1);
            this.$emit('getInformation',info1);
            return;
        },
        display3: function() {
            let res1 = this.res;
            let info1 = this.info;
            if(res1 == '' && info1 == '') {
            }
            else if(res1 == '' && info1 != '') {
                let r1 = info1;
                if(this.judgeNum(r1)) {
                    res1 = r1;
                    info1 = '';
                }
                else {
                    res1 = "invalid input!";
                    info1 = '';
                }
            }
            else if(res1 != '' && info1 == '') {
                if(this.judgeNum(res1)){
                    info1 = '';
                }
                else{
                    res1 = 'invalid input!';
                }
            }
            else if(res1 != '' && info1 != '') {
                if(this.judgeNum(info1))
                {
                    this.calcu(res1,info1);
                    return;
                }
                else {
                    res1 = "invalid input!";
                    info1 = '';
                }
            }
            this.$emit('getResult',res1);
            this.$emit('getInformation',info1);
            return;
        }, 
        display4: function(str) {
            let res1 = this.res;
            let info1 = this.info;
            if(res1 != '' && info1 == '') {
                let tmp = res1[res1.length-1];
                if(tmp == '+' || tmp == '-' || tmp == '×' || tmp == '÷') {

                }
                else {
                    res1 = '';
                }
            }
            info1 = info1+str;
            this.$emit('getResult',res1);
            this.$emit('getInformation',info1);
            return;
        },
        judgeNum: function(str) {
            if(str.length == 0){
		        return false;
            }
            
            if(str[0] == '-' && str.length == 1) {
                return false;
            }

            let flag = new Boolean(false);
            for (var i = str.length - 1; i >= 0; i--) {
                if(str[i] != '.' && str[i] != '-') {
                    if(parseInt(str[i]) >= 0 && parseInt(str[i]) <= 9) {
                    }
                    else {
                        return false;
                    }
                }

                if(str[i] == '.' && i == str.length-1) {
                    return false;
                }
                else if(str[i] == '.' && i != str.length-1)
                {
                    if(flag == false){
                        flag = true;
                    }
                    else{
                        return false;
                    }
                }

                if(str[i] == '+' || str[i] == '×' || str[i] == '÷') {
                    return false;
                }

                if(str[i] == '-' && i != 0) {
                    return false;
                }
            }
            return true;
        },
        calcu: function(res1,info1) {
            // let res1 = this.res;
            // let info1 = this.info;
            let r2 = parseFloat(res1.substring(0,res1.length-1));
            let r3 = parseFloat(info1);
            if(res1[res1.length-1] == '+') {
                let r4 = (r2*10000 + r3*10000)/10000;
                res1 = r4;
                info1 = '';
            }
            else if(res1[res1.length-1] == '-') {
                let r4 = (r2*10000 - r3*10000)/10000;
                res1 = r4;
                info1 = '';
            }
            else if(res1[res1.length-1] == '×') {
                let r4 = (r2*10000) * (r3*10000)/100000000;
                res1 = r4;
                info1 = '';
            }
            else if(res1[res1.length-1] == '÷') {
                if(r3 == '0'){
                    res1 = "The divisor can't be zero!";
                    info1 = '';
                    return;
                }
                let r4 = (r2*10000) / (r3*10000);
                res1 = r4;
                info1 = '';
            }
            this.$emit('getResult',res1);
            this.$emit('getInformation',info1);
            return;
        },
        keyDown: function(e) {
            e.preventDefault();
            if(e.keyCode == 187 && e.shiftKey) {
                this.display2('+');
            }
            else if(e.keyCode == 53 && e.shiftKey) {
                this.display1('%');
            }
            else if(e.keyCode == 192 && e.shiftKey) {
                this.display1('AC');
            }
            else if(e.keyCode == 189 && e.shiftKey) {
                this.display1('+/-');
            }
            else if(e.keyCode == 56 && e.shiftKey) {
                this.display2('×');
            }
            else if(e && (e.keyCode == 48 || e.keyCode == 49 || e.keyCode == 50 || e.keyCode == 51 || e.keyCode == 52 || 
                e.keyCode == 53 || e.keyCode == 54 || e.keyCode == 55 || e.keyCode == 56 || e.keyCode == 57) ) {
                this.display4(e.keyCode - 48);
            }
            else if(e && e.keyCode == 189) {
                this.display2('-');
            }
            else if(e.keyCode == 187 || e.keyCode == 13) {
                this.display3();
            }
            else if(e && e.keyCode == 191) {
                this.display2('÷');
            }
            else if(e && e.keyCode == 190) {
                this.display4('.');
            }
        },
        up: function() {
            this.$emit('getResult','AC');
        }
    }
}
</script>

<style>
    * {
	    box-sizing: border-box;
        outline: none;
    }

    button{
        outline: none;
    }

    body {
        display: flex;
        align-items:center;
        justify-content:center;
    }

    #bottom {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        align-items: stretch;
        justify-content: center;
        align-content: flex-start;
        text-align: center;
        width: 540px;
        height: 500px;
        background-color: green;
        border-bottom-left-radius: 10px;
        border-bottom-right-radius: 10px;
    }

    #bottom .row {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        align-items: center;
        justify-content: center;
        align-content: center;
        width: 540px;
        height: 100px;
        background-color: white;
        
    }

    #bottom .operator1 {
        width:135px;
        height:100px;
        background-color: #E8E8E8;
        border: 1px solid white;
        font-size: 250%;
        text-align: center;
        line-height: 250%;
        font-weight: bold;
    }

    #bottom .operator1:hover {
        background-color: #B0B0B0;
        color:white;
    }

    #bottom .operator2 {
        width:135px;
        height:100px;
        background-color: #FF9933;
        border: 1px solid white;
        font-size: 250%;
        text-align: center;
        line-height: 250%;
        font-weight: bold;
        color:white;
    }

    #bottom .operator2:hover {
        background-color: #B0B0B0;
        color:white;
    }

</style>

