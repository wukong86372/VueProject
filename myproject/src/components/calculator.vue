<template>
    <div id="container">
        <div id="top">
            <div id="tool">
                <p id="btn1">−</p>
                <p id="btn2">□</p>
                <p id="btn3">×</p>
            </div>
            <div id="result">{{result}}
            </div>
            <div id="information">{{information}}
            </div>
        </div>
        <div id="bottom" v-focus="true" @keydown="keyDown($event)" tabindex="-1">
            <div class="row">
                <button @click="display('AC')" class="operator1">AC</button>
                <button @click="display('+/-')" class="operator1">+/−</button>
                <button @click="display('%')" class="operator1">%</button>
                <button @click="display('÷')" class="operator2">÷</button>
            </div>
            <div class="row">
                <button @click="display('7')" class="operator1">7</button>
                <button @click="display('8')" class="operator1">8</button>
                <button @click="display('9')" class="operator1">9</button>
                <button @click="display('×')" class="operator2">×</button>
            </div>
            <div class="row">
                <button @click="display('4')" class="operator1">4</button>
                <button @click="display('5')" class="operator1">5</button>
                <button @click="display('6')" class="operator1">6</button>
                <button @click="display('-')" class="operator2">−</button>
            </div>
            <div class="row">
                <button @click="display('1')" class="operator1">1</button>
                <button @click="display('2')" class="operator1">2</button>
                <button @click="display('3')" class="operator1">3</button>
                <button @click="display('+')" class="operator2">+</button>
            </div>
            <div class="row" style="border-bottom-left-radius: 10px; border-bottom-right-radius: 10px;">
                <button @click="display('0')" class="operator1" style="width: 271px;border-bottom-left-radius: 10px;">0</button>
                <button @click="display('.')" class="operator1">.</button>
                <button @click="display('=')" class="operator2" style="border-bottom-right-radius: 10px;">=</button>
            </div>
        </div>
	</div>
</template>

<script>
export default {
    name:'Calculator',
    data() {
        return {
            result:'',
            information:''
        }
    },
    directives: {
        focus: {
            // 指令的定义
            inserted: function(el, binding) {
                console.log("get Focused");
                if (binding.value) 
                    el.focus();
                else 
                    el.blur();
            },
            componentUpdated: function(el, binding) {
                console.log("get Focused");
                if (binding.value)
                    el.focus();
                else 
                    el.blur();
          }
        }    
    },
    methods: {
       display: function(str) {
            if(str == 'AC') {
                this.result = '';
                this.information = '';
                return;
            }

            if(str == '+/-') {
                if(this.information == '' && this.result != ''){
                    if(this.judgeNum(this.result)){
                        if(this.result[0] == '-') {
                            this.result = this.result.substring(1,this.information.length);
                        }
                        else if(this.result[0] != '-') {
                            this.result = '-' + this.result;
                        }
                    }
                }
                else if(this.information[0] == '-') {
                    this.information = this.information.substring(1,this.information.length);
                }
                else if(this.information[0] != '-') {
                    this.information = '-' + this.information;
                }
                return;
            }

            if(str == '%') {
                if(this.information == '' && this.result == ''){
                    this.result = "invalid input!";
                    return;
                }
                else if(this.information == '' && this.result != '') {
                    if(this.judgeNum(this.result)){
                        this.result = String(parseFloat(this.result)/100);
                        return;
                    }
                }
                else if(this.judgeNum(this.information)){
                    this.information = String(parseFloat(this.information)/100);
                }
                else if(!this.judgeNum(this.information)) {
                    this.result = "invalid input!";
                    this.information = '';
                }
                return;
            }

            if(this.result == '' && this.information == '') {
                if(str == '+' || str == '-' || str == '×' || str == '÷' || str == '=') {
                    return;
                }
            }
            else if(this.result == '' && this.information != '') {
                let r1 = this.information;
                if(str == '+' || str == '-' || str == '×' || str == '÷') {
                    if(this.judgeNum(r1))
                    {
                        this.result = r1 + str;
                        this.information = '';
                        return;
                    }
                    else{
                        this.result = "invalid input!";
                        this.information = '';
                        return;
                    }
                }
                else if(str == '=') {
                    if(this.judgeNum(r1)) {
                        this.result = r1;
                        this.information = '';
                    }
                    else {
                        this.result = "invalid input!";
                        this.information = '';
                    }
                    return;
                }
            }
            else if(this.result != '' && this.information == '') {
                if(str == '+' || str == '-' || str == '×' || str == '÷') {
                    if(this.judgeNum(this.result)){
                        this.result = this.result + str;
                        this.information = '';
                    }
                    else{
                        this.result = 'invalid input!';
                    }
                    return;
                }
                else if(str == '=') {
                    if(this.judgeNum(this.result)){
                        this.information = '';
                    }
                    else{
                        this.result = 'invalid input!';
                    }
                    return;
                }
                else {
                    let tmp = this.result[this.result.length-1];
                    if(tmp == '+' || tmp == '-' || tmp == '×' || tmp == '÷') {

                    }
                    else {
                        this.result = '';
                    }
                }
            }
            else if(this.result != '' && this.information != '') {
                if(str == '=') {
                    if(this.judgeNum(this.information))
                    {
                        this.calcu();
                        return;
                    }
                    else {
                        this.result = "invalid input!";
                        this.information = '';
                        return;
                    }
                }
                else if(str == '+' || str == '-' || str == '×' || str == '÷') {
                    if(this.judgeNum(this.information))
                    {
                        this.calcu();
                        this.result = this.result + str;
                        return;
                    }
                    else {
                        this.result = "invalid input!";
                        this.information = '';
                        return;
                    }	
                }
            }
            this.information = this.information+str;
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
        calcu: function() {
            let res = document.getElementById("result");
            let info = document.getElementById("information");
            let r2 = parseFloat(this.result.substring(0,this.result.length-1));
            let r3 = parseFloat(this.information);
            if(this.result[this.result.length-1] == '+') {
                let r4 = (r2*10000 + r3*10000)/10000;
                this.result = r4;
                this.information = '';
            }
            else if(this.result[this.result.length-1] == '-') {
                let r4 = (r2*10000 - r3*10000)/10000;
                this.result = r4;
                this.information = '';
            }
            else if(this.result[this.result.length-1] == '×') {
                let r4 = (r2*10000) * (r3*10000)/100000000;
                this.result = r4;
                this.information = '';
            }
            else if(this.result[this.result.length-1] == '÷') {
                if(r3 == '0'){
                    this.result = "The divisor can't be zero!";
                    this.information = '';
                    return;
                }
                let r4 = (r2*10000) / (r3*10000);
                this.result = r4;
                this.information = '';
            }
            return;
        },
        keyDown: function(e) {
            e.preventDefault();
            if(e.keyCode == 187 && e.shiftKey) {
                this.display('+');
            }
            else if(e.keyCode == 53 && e.shiftKey) {
                this.display('%');
            }
            else if(e.keyCode == 192 && e.shiftKey) {
                this.display('AC');
            }
            else if(e.keyCode == 189 && e.shiftKey) {
                this.display('+/-');
            }
            else if(e.keyCode == 56 && e.shiftKey) {
                this.display('×');
            }
            else if(e && (e.keyCode == 48 || e.keyCode == 49 || e.keyCode == 50 || e.keyCode == 51 || e.keyCode == 52 || 
                e.keyCode == 53 || e.keyCode == 54 || e.keyCode == 55 || e.keyCode == 56 || e.keyCode == 57) ) {
                this.display(e.keyCode - 48);
            }
            else if(e && e.keyCode == 189) {
                this.display('-');
            }
            else if(e.keyCode == 187 || e.keyCode == 13) {
                this.display('=');
            }
            else if(e && e.keyCode == 191) {
                this.display('÷');
            }
            else if(e && e.keyCode == 190) {
                this.display('.');
            }
        }
    }
}
</script>

<style>
    * {
	    box-sizing: border-box;
    }

    button{
        outline: none;
    }

    body {
        display: flex;
        align-items:center;
        justify-content:center;
    }

    #container {
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        width: 540px;
        height: 650px;
        background-color: grey;
        border-radius:10px;
        box-shadow: 5px 5px 5px #888888;
    }

    #top {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 540px;
        height: 150px;
        background-color: grey;
        border: 1px solid white;
        border-top-left-radius:10px; 
        border-top-right-radius:10px; 
    }

    #top #tool {
        display: flex;
        justify-content: flex-end;
        align-items: flex-start;
        align-content: flex-start;
        width: 540px;
        height: 43px;
        background-color:#E8E8E8;
        border-top-left-radius:10px; 
        border-top-right-radius:10px; 
    }

    #top #tool #btn1 {
        width: 24px;
        height: 43px;
        background-color: #E8E8E8;
        font-size: 130%;
        margin-top: 1%;
        text-align: center;
    }

    #top #tool #btn1:hover {
        background-color: #D0D0D0;
    }

    #top #tool #btn2 {
        width: 24px;
        height: 43px;
        background-color: #E8E8E8;
        font-size: 135%;
        margin-top: 0.5%;
        text-align: center;
    }

    #top #tool #btn2:hover {
        background-color: #D0D0D0;
    }

    #top #tool #btn3 {
        width: 24px;
        height: 43px;
        background-color: #E8E8E8;
        font-size: 130%;
        border-top-right-radius:10px; 
        margin-top: 1%;
        text-align: center;
    }

    #top #tool #btn3:hover {
        background-color:#D0D0D0;
    }

    #top #result{
        width: 540px;
        height: 50px;
        background-color: grey;
        color:white;
        text-align: right;
        text-align: end;
        font-size: 200%;
    }

    #top #information {
        width: 540px;
        height: 57px;
        background-color: grey;
        color:white;
        text-align: right;
        text-align: end;
        font-size: 280%;
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

