<template>
    <p class="panel-tabs">
        <!-- When done -->
        <audio id='thisAudio' autoplay>
            <source src="../assets/done.mp3" type="audio/mpeg">
            <!-- Your browser does not support the audio element. -->
        </audio>
        <!-- When played/paused -->
        <audio id='thisOtherAudio' autoplay>
            <source src="../assets/click.mp3" type="audio/mpeg">
            <!-- Your browser does not support the audio element. -->
        </audio>
        <a v-on:click='change' class="is-active azulito">Pomodoro
                    <span id='first-tag' class='tag'>0</span>
        </a>

        <a v-on:click='change' class='verdecito'>Short Break
                    <span id='second-tag' class='tag'>0</span>
        </a>

        <a v-on:click='change' class='verdecito'>Long Break
                    <span id='third-tag' class='tag'>0</span>
        </a>

    </p>
</template>

<script>

export default {
    name: 'controlPanel',
    data(){
        return{
            pomodoros: {
                completed: 0,
                shortBreaks: 0,
                longBreaks: 0
            }
        }
    },
    props:{
        'currentCount': Function,
        'interrupting': Boolean,
        'changedStop': Function

    },
    methods:{
        updateTag(){
            let object = localStorage.getItem('pomodoroOfMine') ? JSON.parse(localStorage.getItem('pomodoroOfMine')).pomodoros : this.pomodoros;
            /* console.log(object); */ // Why the fuck does this output 'observer'?
            if(object){
                this.pomodoros.completed = object.completed;
                this.pomodoros.shortBreaks = object.shortBreaks;
                this.pomodoros.longBreaks = object.longBreaks;
            }

            /* Get elements */
            let tag1 = document.getElementById('first-tag');
            let tag2 = document.getElementById('second-tag');
            let tag3 = document.getElementById('third-tag');
            tag1.innerHTML = object.completed;
            tag2.innerHTML = object.shortBreaks;
            tag3.innerHTML = object.longBreaks;


        },
        change(e){
            /* Here I think we should do the 'interrupting' check
            ** If not interrupting... */
            if(!this.interrupting){
                /* Reseting selection before selecting new one */
                let as = document.querySelectorAll('p.panel-tabs > a');
                for (let i = 0; i < as.length; i++) {
                    const el = as[i];
                    el.classList.remove('is-active');
                }

                /* Actually selecting the one clicked */
                e.target.classList.toggle('is-active');

                /* Now picking a color */
                let currentTab = e.target.childNodes[0].nodeValue.trim();
                switch (currentTab) {
                    case 'Pomodoro':
                        this.currentCount(1500);
                        break;
                    case 'Short Break':
                        this.currentCount(2);
                        break;
                    case 'Long Break':
                        this.currentCount(900);
                        break;
                    default:
                        break;
                }
            }else{
                if(confirm('Are you sure you want to interrupt the countdown already started?')){
                    /* Reseting selection before selecting new one */
                    let as = document.querySelectorAll('p.panel-tabs > a');
                    for (let i = 0; i < as.length; i++) {
                        const el = as[i];
                        el.classList.remove('is-active');
                    }
                    /* Actually selecting the one clicked */
                    e.target.classList.toggle('is-active');
                    this.changedStop();

                    /* Now picking a color */
                    let currentTab = e.target.childNodes[0].nodeValue.trim();
                    switch (currentTab) {
                        case 'Pomodoro':
                            this.currentCount(1500);
                            break;
                        case 'Short Break':
                            this.currentCount(2);
                            break;
                        case 'Long Break':
                            this.currentCount(900);
                            break;
                        default:
                            break;
                    }
                }
            }

            this.updateTag();


        }
    },
    mounted(){
        this.updateTag();
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    .panel-tabs a{
        height: 700;
        font-size: 1.5em;
        font-family: 'STIX Two Text', serif;
    }
    .azulito{
        color:#2c7da0 !important;
    }
    .verdecito{
        color:#52b788 !important;
    }
    .panel-tabs{
        align-items: center;
    }
    .tag{
        position:relative;
        top:-2px;
        pointer-events: none;
    }

</style>
