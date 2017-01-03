<template>
    <div id="container">
          <draggable :list="cards" :options="{group:'solitaire'}" :move="move" @end="moveCard" :id="'spot-'+number" class="draggableArea">
            <!-- <card v-for="card in cards" :instance="card"></card> -->
          <ul>
            <template v-for="(card, index) in cards">
              <li class="card back" v-if="card.flipped" :id="index">*</li>
              <li class="card rank-7" :class="card.suit.name" v-else :id="index" :key="index">
                  <span class="rank" v-text="card.entity.rep"></span>
                  <span class="suit" v-text="card.suit.rep"></span>
              </li>
            </template>
          </ul>
          </draggable>
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
        z-index: -1;
        display:table-cell;
        /*list-style-type:none;*/
    }
    #container ul li {
        /*box-sizing: content-box;*/
        /*position:relative;*/
        /*top:-12px;*/
    }

    #container ul div li:nth-child(1)  { left: 0;    top: 0; z-index: 599;}
    #container ul li:nth-child(2)  { left: 0;    top: -85px; z-index: 600;}
    #container ul li:nth-child(3)  { left: 0;    top: -170px; z-index: 601;}
    #container ul li:nth-child(4)  { left: 0;    top: -255px; z-index: 602;}
    #container ul li:nth-child(5)  { left: 0;    top: -340px; z-index: 603;}
    #container ul li:nth-child(6)  { left: 0;    top: -425px; z-index: 604;}
    #container ul li:nth-child(7)  { left: 0;    top: -510px; z-index: 605;}
    #container ul li:nth-child(8)  { left: 0;    top: -595px; z-index: 606;}
    #container ul li:nth-child(9)  { left: 0;    top: -680px; z-index: 607;}
    #container ul li:nth-child(10)  { left: 0;    top: -765px; z-index: 608;}
    #container ul li:nth-child(11)  { left: 0;    top: -850px; z-index: 609;}

    .draggableArea {
      min-height: 100px;
      min-width: 100px;
      border:2px solid rgb(255,133,100);
    }
</style>
<script>

    import draggable from 'vuedraggable'

    export default{
        data(){
            return{
              from: null,
              to: null,
              card: null
            }
        },
        components:{
                draggable
        },
        props: ['cards','number'],
        methods: {
          moveCard: function (event) {
            window.Event.$emit('movement',this.generateMovementInstructions(event))
            this.resetFromTo()
          },
          move: function (event) {
            // console.log(event)
            this.from = this.detectSpotNumber(event.from.id)
            this.to = this.detectSpotNumber(event.to.id)
            this.card = event.draggedContext.element
            return false
          },
          generateMovementInstructions: function (event) {
            // console.log(this.card)
            return {
              card: this.card,
              from: this.from,
              to: this.to
            }
          },
          detectSpotNumber: function (string) {
              return parseInt(string.replace('spot-',''))
          },
          resetFromTo: function () {
            this.from = null,
            this.to = null,
            this.card = null
          }
        },
        computed: {
          rev: function () {
            return this.cards.reverse()
          }
        }
    }
</script>
