<template>

    <div class="el">

        <div class="title">Tasks...</div>

        <button v-on:click='showForm' class="button is-medium is-fullwidth">
            <i class="fas fa-plus-circle"></i>
            Add task.
        </button>

        <!-- <form v-if='show' class="box myBox">
            <div class="field">
                <label class="label">Email</label>
                <div class="control">
                <input class="input" type="email" placeholder="e.g. alex@example.com">
                </div>
            </div>

            <div class="field">
                <label class="label">Password</label>
                <div class="control">
                <input class="input" type="password" placeholder="********">
                </div>
            </div>

            <button class="button is-primary">Sign in</button>
        </form> -->

    </div>

</template>

vue <script>
export default {
    name: 'taskList',
    data(){
        return{
            show: false
        }
    },
    methods:{
        showForm(){
            this.show = !this.show;
            let body = document.getElementsByName('body');
            console.log(body);
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    .el{
        position:relative;
        /* positioning input */
        top:-140px;
        min-width:33vw;
    }
    .title{
        color:white;
        font-family: 'Playfair Display', serif;
        border-bottom: 1px solid honeydew;
        width:100%;
        display:flex;
        font-size:1.2em;
    }
    .icon{
        color:black !important;
    }
    .myBox{
        position:absolute;
        top:100px;

    }


</style>
