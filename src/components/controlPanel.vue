<template>
    <p class="panel-tabs">
        <a v-on:click='change' class="is-active">Pomodoro</a>
        <a v-on:click='change'>Short Break</a>
        <a v-on:click='change'>Long Break</a>
    </p>
</template>

<script>


export default {
    name: 'controlPanel',
    props:{
        'currentColor': Function,
        'currentCount': Function,
        'interrupting': Boolean,
        'changedStop': Function

    },
    methods:{
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
                let currentTab = e.target.innerHTML;
                switch (currentTab) {
                    case 'Pomodoro':
                        this.currentColor('#3A4750');
                        this.currentCount(1500);
                        break;
                    case 'Short Break':
                        this.currentColor('#3F72AF');
                        this.currentCount(1);
                        break;
                    case 'Long Break':
                        this.currentColor('#878ECD');
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
                    let currentTab = e.target.innerHTML;
                    switch (currentTab) {
                        case 'Pomodoro':
                            this.currentColor('#3A4750');
                            this.currentCount(1500);
                            break;
                        case 'Short Break':
                            this.currentColor('#3F72AF');
                            this.currentCount(1);
                            break;
                        case 'Long Break':
                            this.currentColor('#878ECD');
                            this.currentCount(900);
                            break;
                        default:
                            break;
                    }
                }
            }




        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    .panel-tabs a{
        color:white;
        height: 700;
        font-size: 1.2em;
    }

</style>
