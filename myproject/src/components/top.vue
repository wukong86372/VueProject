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
        <Bottom v-bind:res=this.result v-bind:info=this.information @getResult="showResult" @getInformation="showInfomation"></Bottom>
    </div>
</template>

<script>
import Bottom from './bottom'
export default {
    name:'Top',
    components: {
        Bottom
    },
    methods: {
        showResult: function(res1) {
            this.result = res1;
        },
        showInfomation: function(info1) {
            console.log(info1);
            this.information = info1;
        }
    },
    data() {
        return {
            result:'',
            information:''
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

</style>


