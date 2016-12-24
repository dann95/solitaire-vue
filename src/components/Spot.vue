<template>
    <div id="container">
        <ul class="deck">
            <li v-for="card in cards"><card :instance="card"></card></li>
        </ul>
    </div>
</template>
<style scoped>
    #container{
        background-color:#90FF0E;
        border:1px solid #583BFF;
        width:130px;
        height:200px;
    }
    #container ul {
        display:table-cell;
        list-style-type:none;
    }
    #container ul li {
        position:relative;
        top:-12px;
    }
</style>
<script>
    import Card from './Card'
    export default{
        data(){
            return{

            }
        },
        components:{
                Card
        },
        props: ['cards'],
        methods: {

        }
    }
</script>
