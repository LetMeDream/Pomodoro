<template>

    <div class="container-mine">

        <control-panel :changedStop='changedStop' :interrupting="interrupting" :count='refCount' :currentColor='currentColor' :currentCount='currentCount' ></control-panel>

        <div class="countdown-container">
            <div class="countdown">
                {{ minutes }}:<span v-if='seconds<10'>0</span>{{seconds}}
            </div>
        </div>

        <button @click='startCount' class="button is-medium myBtn">
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
            this.counting = !this.counting;

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
                            mybtn.classList.add('finished');
                            this.done=true;
                            this.interrupting=false;
                            mybtn.innerHTML = 'All done!';
                            let interval = setInterval(()=>{
                                mybtn.innerHTML = 'Reset.';
                                /* Reseting */
                                this.counting = !this.counting;
                                mybtn.classList.remove('finished');
                                /* Countdown is done */
                            }, 2000);
                            clearInterval(interval);
                        }
                    } , 1000);
            }

        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    .container-mine{
        background-color: rgba(255, 255, 255, 0.489);
        border-radius:8px;
        height: 210px;
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
        min-width:108px;
        box-shadow: none;
        transition: 0.5s all ease-in;
    }
    .myBtn:focus{
        box-shadow: none;
        border-color:transparent;
    }
    .myBtn:active{
        box-shadow: none;
    }
    .myBtnClicked{
        transform: translateY(2px) translateX(2px);
    }
    .finished{
        color:green;
    }


</style>
