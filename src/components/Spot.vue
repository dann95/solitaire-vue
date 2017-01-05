<template>
    <div id="container">
          <draggable :list="cards" :options="{group:'solitaire'}" :move="move" @end="moveCard" :id="'spot-'+number" class="draggableArea" element="ul" :class="{'wellAlign': 'number == 13' }">
              <div v-if="number == 13" id="showcase">
                <template v-for="(card,index) in well.currents">
                    <li class="card rank-7 wellAlign" :class="card.suit.name" :id="index" :key="index">
                        <span class="rank" v-text="card.entity.rep"></span>
                        <span class="suit" v-text="card.suit.rep"></span>
                    </li>
                </template>
                <button v-if="number == 13 && cards.length > 0" @click="onWellClick">ask card</button>
              </div>
            <template v-for="(card, index) in cards" v-if="number != 13">
              <li class="card back deckAlign" v-if="card.flipped" :id="index">*</li>
              <li class="card rank-7 deckAlign" :class="card.suit.name" v-else :id="index" :key="index">
                  <span class="rank" v-text="card.entity.rep"></span>
                  <span class="suit" v-text="card.suit.rep"></span>
              </li>
            </template>
            <template v-for="(card, index) in visibleWellCards" v-if="number == 13">
                <!-- display if displayable(index)  -->
                <li class="card back" :id="index">*</li>
            </template>
            <br class="emptyMin">
          </draggable>
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
                  currents:[],
              }
            }
        },
        computed: {
            wellHaveCards: function () {
                return true
            }
        },
        components:{
                draggable
        },
        props: ['cards','number'],
        methods: {
            // showableWellCard: function ( index ) {
            //     return (this.well.currents.length > 0) ? this.well.currents.map(function (c) { return { number: c.entity.number } }).filter(function (card) { return (this.cards[index].entity.number == card.number ) }) : true
            // },
            onWellClick: function () {
                // let x = this.cards
                console.log(this)
                let newCard = this.cards.pop()
                console.log(newCard)
                console.log('~~~~~~~~')
                this.well.currents.push(newCard)
                console.log(this.well.currents)
                // var card = this.cards.pop()
                // console.log(this.)
                // this.well.currents.push(card)
                // console.log(this.well.currents)
            },
            moveCard: function (event) {
                window.Event.$emit('movement',this.generateMovementInstructions(event))
                this.resetFromTo()
            },
            move: function (event) {
                // console.log(event.dragged.id)
                this.from = this.detectSpotNumber(event.from.id)
                this.to = this.detectSpotNumber(event.to.id)
                this.cardIndex = event.dragged.id
                return false
            },
            generateMovementInstructions: function (event) {
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
                return this.cards.slice(this.well.currents.length)
            }
        }
    }
</script>
