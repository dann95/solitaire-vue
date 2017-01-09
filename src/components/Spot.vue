<template>
    <div id="container">
          <template v-if="number == 13">
              <ul>
                  <li v-for="card in cards" class="card rank-7 back" @click="onWellClick">*</li>
              </ul>
              <draggable
              :list="visibleWellCards"
              element="ul"
              :options="{group: 'solitaire'}"
              :move="move"
              @end="moveCard"
              class="draggableArea"
              :id="'spot-' + number"
              >
                  <li
                  v-for="card in visibleWellCards"
                  class="card rank-7"
                  :class="card.suit.name"
                  :id="well.current">
                      <span class="rank" v-text="card.entity.rep"></span>
                      <span class="suit" v-text="card.suit.rep"></span>
                  </li>
                  <br class="emptyMin">
              </draggable>
          </template>
          <template v-else>
              <draggable
              :list="cards"
              :options="{group: 'solitaire'}"
              :move="move"
              @end="moveCard"
              element="ul"
              class="draggableArea"
              :id="'spot-' + number">
                <template v-for="(card, index) in cards">
                    <li
                    v-if="card.flipped"
                    class="card back deckAlign">*</li>
                    <li
                    v-else
                    class="card rank-7 deckAlign"
                    :class="card.suit.name"
                    :id="index"
                    :key="index"
                    >
                        <span class="rank" v-text="card.entity.rep"></span>
                        <span class="suit" v-text="card.suit.rep"></span>
                    </li>
                </template>
                <br class="emptyMin">
              </draggable>
          </template>
    </div>
</template>
<style scoped>
    #showcase {
        /*position: absolute;*/
        display: inline-block;

    }
    #draggableShow {
        left:100px;
        /*position: absolute;*/
    }
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
        box-sizing: content-box;
        position:relative;
        /*top:-10px;*/
    }

    #container ul li.wellAlign:nth-child(1)  { left: 0;    top: 0; z-index: 599;}
    #container ul li.wellAlign:nth-child(2)  { left: 0;    top: -109px; z-index: 600;}
    #container ul li.wellAlign:nth-child(3)  { left: 0;    top:-218px; z-index: 601;}
    #container ul li.deckAlign:nth-child(1)  { left: 0;    top: 0; z-index: 599;}
    #container ul li.deckAlign:nth-child(2)  { left: 0;    top: -85px; z-index: 600;}
    #container ul li.deckAlign:nth-child(3)  { left: 0;    top: -170px; z-index: 601;}
    #container ul li.deckAlign:nth-child(4)  { left: 0;    top: -255px; z-index: 602;}
    #container ul li.deckAlign:nth-child(5)  { left: 0;    top: -340px; z-index: 603;}
    #container ul li.deckAlign:nth-child(6)  { left: 0;    top: -425px; z-index: 604;}
    #container ul li.deckAlign:nth-child(7)  { left: 0;    top: -510px; z-index: 605;}
    #container ul li.deckAlign:nth-child(8)  { left: 0;    top: -595px; z-index: 606;}
    #container ul li.deckAlign:nth-child(9)  { left: 0;    top: -680px; z-index: 607;}
    #container ul li.deckAlign:nth-child(10)  { left: 0;    top: -765px; z-index: 608;}
    #container ul li.deckAlign:nth-child(11)  { left: 0;    top: -850px; z-index: 609;}
    #container ul li:nth-child(1)  { left: 0;    top: 0; z-index: 599;}
    #container ul li:nth-child(2)  { left: 0;    top: -107px; z-index: 600;}
    #container ul li:nth-child(3)  { left: 0;    top: -214px; z-index: 601;}
    #container ul li:nth-child(4)  { left: 0;    top: -321px; z-index: 602;}
    #container ul li:nth-child(5)  { left: 0;    top: -428px; z-index: 603;}
    #container ul li:nth-child(6)  { left: 0;    top: -535px; z-index: 604;}
    #container ul li:nth-child(7)  { left: 0;    top: -642px; z-index: 605;}
    #container ul li:nth-child(8)  { left: 0;    top: -749px; z-index: 606;}
    #container ul li:nth-child(9)  { left: 0;    top: -856px; z-index: 607;}
    #container ul li:nth-child(10)  { left: 0;    top: -963px; z-index: 608;}
    #container ul li:nth-child(11)  { left: 0;    top: -1070px; z-index: 609;}
    #container ul li:nth-child(12)  { left: 0;    top: -1177px; z-index: 609;}
    #container ul li:nth-child(13)  { left: 0;    top: -1284px; z-index: 609;}
    #container ul li:nth-child(14)  { left: 0;    top: -1391px; z-index: 609;}
    #container ul li:nth-child(15)  { left: 0;    top: -1498px; z-index: 609;}
    #container ul li:nth-child(16)  { left: 0;    top: -1605px; z-index: 609;}

    .emptyMin {
        line-height:200px;
    }
    .draggableArea {
      min-width: 90px;
      border:2px solid purple;
    }
</style>
<script>

    import draggable from 'vuedraggable'

    export default{
        data(){
            return{
              from: null,
              to: null,
              cardIndex: null,
              well: {
                  current:null,
                  last:null
              }
            }
        },
        components:{
                draggable
        },
        props: ['cards','number'],
        methods: {
            onWellClick: function () {
                if(this.well.current == null){
                    this.well.current = 0
                    this.well.last = 0
                    return
                }
                if(this.well.current == this.cards.length - 1){
                    this.well.current = 0
                    this.well.last = 0
                    return
                }
                this.well.last = this.well.current
                this.well.current += 1
                return
            },

            pushNextCardToShowcase: function (index = 0) {
                this.well.currents.push(index)
            },

            cleanCurrents: function () {

            },
            moveCard: function (event) {
                window.Event.$emit('movement',this.generateMovementInstructions(event))
                this.resetFromTo()
            },
            move: function (event) {
                this.from = this.detectSpotNumber(event.from.id)
                this.to = this.detectSpotNumber(event.to.id)
                this.cardIndex = event.dragged.id
                return false
            },
            generateMovementInstructions: function (event) {
                console.log({
                    index: this.cardIndex,
                    from: this.from,
                    to: this.to
                })
                return {
                    index: this.cardIndex,
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
            visibleWellCards: function () {
                var arr = []
                if(this.well.current == null){
                    return arr
                }
                arr.push(this.cards[this.well.current])
                return arr
            }
        }
    }
</script>
