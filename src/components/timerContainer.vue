<template>

    <div class="container-mine">

        <control-panel :changedStop='changedStop' :interrupting="interrupting" :count='refCount' :currentColor='currentColor' :currentCount='currentCount' ></control-panel>

        <div class="countdown-container">
            <div class="countdown">
                {{ minutes }}:<span v-if='seconds<10'>0</span>{{seconds}}
            </div>
        </div>

        <button @click='startCount' id='firstBtn' class="button is-rounded is-dark is-medium myBtn">
            <p v-if='counting && (count!=refCount)'>Stop</p>
            <p v-else-if='count!=refCount'>Continue</p>
            <p v-else>Start</p>
        </button>




    </div>


</template>

<script>
import ControlPanel from './controlPanel.vue'



export default {
    name: 'timerContainer',
    components:{
        ControlPanel
    },
    props:{
        'currentColor': Function
    },
    data() {
        return {
            count: 1500,
            refCount: 1500,
            counting: false,
            done:false,
            /* Just to know if we are INTERRUPTING an already started countdown by changing Tabs*/
            interrupting:false
        }
    },
    computed:{
        minutes:function(){
            return Math.floor(this.count/60)
        },
        seconds:function(){
            return Math.floor((this.count)%60)
        },
        colorButton:function(){
            if(!this.interrupting){
                if(this.count=='1'){
                    return 2;
                }
            }
            return 1;
        }

    },
    watch:{
        done:function(){
            setTimeout(() => {
                let mybtn = document.getElementById('firstBtn');
                console.log(mybtn);
                mybtn.classList.remove('is-loading');
                /* Let's here finish doing all the reseting such as...  */
                /* mybtn.innerHTML = 'Start'; */
                this.done = false;
                location.reload();

            }, 3500);
        }
    },
    methods: {
        /* Here we receive Count (25, 5 or 15 mins) from Child component */
        currentCount(newCount){
            this.refCount = newCount;
            this.count = newCount;
        },
        /* This one just makes sure countdown resets AND stops whenever we change an already started countdown */
        changedStop(){
            this.interrupting = false;
        },
        /* Pretty obviously, here we start the count(down) */
        startCount(){
            this.interrupting = true;
            let mybtn = document.querySelector('.myBtn');
            mybtn.classList.toggle('myBtnClicked')
            !this.done ? this.counting = !this.counting : this.counting = true ;

            /* Do we need a reset maybe? */
            if(this.done){
                this.count = this.refCount;
                mybtn.innerHTML = 'Start';
            }

            if(this.count>0){
                    let xx = setInterval(()=>
                        {
                            if(this.counting &&  this.count>0 && this.interrupting){
                                this.count=this.count-1;
                            }else{
                                clearInterval(xx);
                            }
                            if(this.count === 0){
                                this.done=true;
                                this.interrupting=false;
                                mybtn.innerHTML ="Let's count that one in.";
                                setTimeout(() => {
                                    mybtn.innerHTML  += '.';
                                }, 750);
                                setTimeout(() => {
                                    mybtn.innerHTML  += '.';
                                }, 1500);
                                setTimeout(()=>{
                                    /* Reseting */
                                    this.counting = false;
                                    this.count = this.refCount;
                                    mybtn.classList.add('is-loading');
                                    /* Countdown is done */
                                }, 2000);
                                clearInterval(xx);
                            }
                        }
                    , 1000);
            }

        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    .stop{
        background-color:#BD4B4B;
    }
    .stop:hover,
    .stop:focus{
        background-color:#974848;
    }

    .container-mine{
        -webkit-box-shadow: 3px 3px 5px 0px rgba(0,0,0,0.75);
        -moz-box-shadow: 3px 3px 5px 0px rgba(0,0,0,0.75);
        box-shadow: 3px 3px 5px 0px rgba(0,0,0,0.75);
        background-color:rgba(208, 208, 208, 0.645);
        border-radius:8px;
        /* height: 210px; */
        /* height: 30px; */
        min-width: 25vw;
        max-width: 50vw;

        display:flex;
        flex-direction: column;
    }
    .container-mine button{
        align-self: center;
    }
    .countdown{
        color:white;
        font-size: 4.20em;
        font-family:'Desconsolata';
    }
    span{
        font-family:'Desconsolata';
    }
    .myBtn{
        margin-bottom: 12px;
        min-width:108px;
        box-shadow: none;
        transition: 0.1s all ease-in;
    }
    .myBtn:focus{
        box-shadow: none;
        border-color:transparent;
    }
    .myBtn:active{
        box-shadow: none;
    }
    .myBtnClicked{
       transform: translateY(2px)
    }
    .finished{
        color:green;
    }


</style>
