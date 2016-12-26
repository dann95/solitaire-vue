<template>
    <div id="container">
        <ul class="deck">
          <draggable :list="cards" :options="{group:'solitaire'}" :move="move" @end="moveCard" :id="'spot-'+number" class="draggableArea">
            <card v-for="card in cards" :instance="card"></card>
          </draggable>
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
    .draggableArea {
      min-height: 100px;
      min-width: 100px;
      border:2px solid rgb(255,133,100);
    }
</style>
<script>

    import Card from './Card'
    import draggable from 'vuedraggable'

    export default{
        data(){
            return{
              from: null,
              to: null
            }
        },
        components:{
                Card,
                draggable
        },
        props: ['cards','number'],
        methods: {
          moveCard: function (event) {
            window.Event.$emit('movement',this.generateMovementInstructions(event))
            this.resetFromTo()
          },
          move: function (event) {
            this.from = this.detectSpotNumber(event.from.id)
            this.to = this.detectSpotNumber(event.to.id)
            return false
          },
          generateMovementInstructions: function (event) {
            return {
              card: event.item.__vue__.instance,
              from: this.from,
              to: this.to
            }
          },
          detectSpotNumber: function (string) {
              return parseInt(string.replace('spot-',''))
          },
          resetFromTo: function () {
            this.from = null,
            this.to = null
          }
        }
    }
</script>
