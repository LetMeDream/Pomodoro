<template>

    <div class="outter-container">

        <nav class="navbar">
            <div class="navbar-brand">
                <a class="navbar-item" href="https://bulma.io">
                    Pomodoro timer of mine
                </a>
                <div class="navbar-burger" data-target="navbarExampleTransparentExample">
                <span></span>
                <span></span>
                <span></span>
                </div>
            </div>

            <div id="navbarExampleTransparentExample" class="navbar-menu">
                <div class="navbar-start">

                </div>

                <div class="navbar-end">
                    <div class="navbar-item has-dropdown is-hoverable">
                        <a class="navbar-link">
                            <i class="fas fa-sliders-h"></i>
                            Customize
                        </a>
                        <div class="navbar-dropdown">
                        <a class="navbar-item">
                            <i class="far fa-clock"></i>&nbsp;
                            Concentration Lv.
                        </a>
                        <a class="navbar-item">
                            <i class="fas fa-headphones-alt"></i>&nbsp;
                            Background music and alarm
                        </a>
                        <a class="navbar-item">
                            <i class="fas fa-redo-alt"></i>&nbsp;
                            Autoplay
                        </a>
                        <hr class="navbar-divider">
                        <a class="navbar-item">
                            Once upon a midnight dreary
                        </a>
                        </div>
                    </div>
                    <div class="navbar-item">
                        <a target="_blank" href="https://www.youtube.com/watch?v=V7CO3dYfKHs">
                        <i class="fas fa-heart"></i>
                        ~Kill myself~</a></div>
                </div>
            </div>
        </nav>

        <div class="container-mine">

            <aside class="leftie">
                <control-panel
                    :changedStop='changedStop'
                    :interrupting="interrupting"
                    :count='refCount'
                    :currentCount='currentCount' >
                </control-panel>
                <div class="countdown-container" :class='{ bordeverde : refCount != 1500 }'>
                    <div class="countdown" :class='{ verdecitx : refCount != 1500 , azulitx : refCount == 1500 }'>
                        {{ minutes }}:<span v-if='seconds<10'>0</span>{{seconds}}
                    </div>
                </div>
                <button @click='startCount' id='firstBtn' class="button is-rounded is-medium myBtn"
                        :class='{ azulito : refCount == 1500 , verdecito : refCount != 1500}'
                >
                    <div v-if='!done' class="aint-done">
                        <p v-if='counting && (count!=refCount)'>Stop</p>
                        <p v-else-if='count!=refCount'>Continue</p>
                        <p v-else>Start</p>
                    </div>
                    <div v-else class="aint-done">
                        <p>Let's count that one in.</p>
                    </div>
                </button>
            </aside>

            <aside class="rightie">

                <div class="containerRightie">
                    <div class="p-2 title is-3">
                        Tasks <span class="tag">0</span>
                    </div>

                    <div :click='adding' class="task-creator p-4 m-2">
                        <i class="fas fa-plus myIcon"></i>
                        Add in here a new task
                    </div>

                </div>


            </aside>


        </div>

    </div>




</template>

<script>
    import ControlPanel from './controlPanel.vue'
    export default {
        name: 'timerContainer',
        components:{
            ControlPanel
        },
        data() {
            return {
                count: 1500,
                refCount: 1500,
                counting: false,
                done:false,
                /* Just to know if we are INTERRUPTING an already started countdown by changing Tabs*/
                interrupting:false,
                /* Are we adding? */
                addin: false
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
                    mybtn.classList.remove('is-loading');
                    /* Let's here finish doing all the reseting such as...  */
                    /* console.log(mybtn);
                    console.log(mybtn.children); */

                    this.done = true;
                    /* Let's have localStorage save our asses here */
                    /* If localStorage[''] doesn't have our object */
                    if(!localStorage.getItem('pomodoroOfMine')){
                        console.log('lets then create it');
                        let object = JSON.stringify({
                                app: 'Vue 3',
                                pomodoros: {
                                    completed: 0,
                                    shortBreaks: 0,
                                    longBreaks: 0
                                }
                            });
                        localStorage.setItem('pomodoroOfMine', object);
                        let pomodorosOfMine = JSON.parse(localStorage.getItem('pomodoroOfMine'));
                        console.log(pomodorosOfMine);
                        switch (this.refCount) {
                            case 2:
                                pomodorosOfMine.pomodoros.shortBreaks+=1;
                                localStorage.setItem('pomodoroOfMine', JSON.stringify(pomodorosOfMine));
                                break;
                            case 900:
                                pomodorosOfMine.pomodoros.longBreaks+=1;
                                localStorage.setItem('pomodoroOfMine', JSON.stringify(pomodorosOfMine));
                                break;
                            case 1500:
                                pomodorosOfMine.pomodoros.completed+=1;
                                localStorage.setItem('pomodoroOfMine', JSON.stringify(pomodorosOfMine));
                                break;
                            default:
                                break;
                        }
                    } else{
                        /* if localStorage[''] does have our object */
                        let pomodorosOfMine = JSON.parse(localStorage.getItem('pomodoroOfMine'));
                        console.log(pomodorosOfMine);
                        switch (this.refCount) {
                            case 2:
                                pomodorosOfMine.pomodoros.shortBreaks+=1;
                                localStorage.setItem('pomodoroOfMine', JSON.stringify(pomodorosOfMine));
                                break;
                            case 900:
                                pomodorosOfMine.pomodoros.longBreaks+=1;
                                localStorage.setItem('pomodoroOfMine', JSON.stringify(pomodorosOfMine));
                                break;
                            case 1500:
                                pomodorosOfMine.pomodoros.completed+=1;
                                localStorage.setItem('pomodoroOfMine', JSON.stringify(pomodorosOfMine));
                                break;
                            default:
                                break;
                        }
                    }


                    /* location.reload(); */

                }, 3500);
            }
        },
        methods: {
            adding(){
                this.addin = !this.addin;
                console.log('sex')
            },
            alarm(){
                let audio = document.getElementById('thisAudio');
                audio.play();
            },
            clicked(){
                let audio = document.getElementById('thisOtherAudio');
                audio.play();
            },
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
                this.clicked();
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
                                    this.alarm();
                                    this.done=true;
                                    this.interrupting=false;
                                    console.log(mybtn.children[0].children[0]);
                                    /*  */
                                    setTimeout(() => {
                                        mybtn.children[0].children[0].innerHTML  += '.';
                                        console.log(mybtn.children[0].children[0]);
                                    }, 750);
                                    setTimeout(() => {
                                        mybtn.children[0].children[0].innerHTML  += '.';
                                        console.log(mybtn.children[0].children[0]);
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

    .myIcon{
        position:absolute;
        left:18px;
        bottom:20px;
    }
    .containerRightie{
        width:100%;
    }
    .task-creator{
        position:relative;
        display:flex;
        justify-content: center;
        border:1px dashed black;
        border-radius: .8rem;
        background-color:rgba(201, 201, 201,.5);
        cursor: pointer;
        -webkit-box-shadow: .1px .6px 6px .1px #000000;
        box-shadow: .1px .6px 6px .1px #000000;

        transition: .3s all ease;
    }
    .task-creator:hover{
        transform: translateY(-1px)
    }
    .stop{
        background-color:#BD4B4B;
    }
    .stop:hover,
    .stop:focus{
        background-color:#974848;
    }

    .outter-container{
        min-width: 90vw;
    }
    .navbar{
        background-color:rgba(255, 254, 254, 0.877);
    }

    .container-mine{
        -webkit-box-shadow: 3px 3px 5px 0px rgba(0,0,0,0.75);
        -moz-box-shadow: 3px 3px 5px 0px rgba(0,0,0,0.75);
        box-shadow: 3px 3px 5px 0px rgba(0,0,0,0.75);
        background-color:rgba(255, 254, 254, 0.877);
        border-radius:0 0 8px8px;

        display:flex;
        height:80vh;

    }
    .leftie{
        width:80%;
        border-right: 1px solid black;
    }
    .container-mine button{
        align-self: center;
    }
    .countdown{
        color:rgb(96, 105, 118);
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
    .azulito{
        color:white !important;
        background-color:#2c7da0 !important;
        transition: .4s all ease;
    }
    .azulitx{
        transition: .4s all ease;
        color:#2c7da0 !important;
    }
    .verdecito{
        color:white !important;
        background-color:#07ae60 !important;
    }
    .verdecitx{
        transition: .4s all ease;
        color:rgb(7, 174, 96) !important;
    }
    .bordeverde{
        transition: .4s all ease;
        border:2px solid rgba(7, 174, 96, 0.645) !important;
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
    .leftie{
        display:flex;
        flex-direction: column;
        justify-content: space-between;
    }
    .rightie{
        flex:1;
    }
    .countdown-container{
        border:2px solid rgba(23, 133, 206, 0.645);
        width:310px;
        height: 310px;
        border-radius: 100%;
        align-self: center;

        display:flex;
        justify-content: center;
        align-items: center;
    }


</style>
