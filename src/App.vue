<template>
    <div id="solitaire" class="playingCards twoColours rotateHand">
        <div id="suit-decks">
            <div class="container">
                <ul class="spots-row">
                    <li class="spot-container">
                        <spot flipped="0" number="1" :cards="session.spots[1]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="0" number="2" :cards="session.spots[2]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="0" number="3" :cards="session.spots[3]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="0" number="4" :cards="session.spots[4]"></spot>
                    </li>
                </ul>
                <div id="well">
                    <spot flipped="16" number="13" :cards="session.spots[13]"></spot>
                </div>
                <div id="buttons">
                    <button @click="newGame()">New Game</button>
                    <button>Help</button>
                    <button>Restart</button>
                    Moves: {{ session.scores.moves }} | Score: {{ session.scores.score }}
                </div>
            </div>
        </div>
        <div id="decks">
            <div class="container">
                <ul class="spots-row">
                    <li class="spot-container">
                        <spot flipped="0" number="5" :cards="session.spots[5]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="1" number="6" :cards="session.spots[6]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="2" number="7" :cards="session.spots[7]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="3" number="8" :cards="session.spots[8]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="4" number="9" :cards="session.spots[9]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="5" number="10" :cards="session.spots[10]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="6" number="11" :cards="session.spots[11]"></spot>
                    </li>
                    <li class="spot-container">
                        <spot flipped="7" number="12" :cards="session.spots[12]"></spot>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import Spot from './components/Spot'
    export default {
        name: 'app',
        components: {
            Spot
    },
    data: function () {
        return {
            defaults: {
                rules: {
                    well: [
                        function (from, to) {
                            return false
                        }
                    ],
                    suitDecks: [
                        function (from, to) {

                        },
                        function (from, to) {

                        },
                        function (from, to) {

                        },
                        function (from, to) {

                        },
                        function (from, to) {

                        },
                        function (from, to) {

                        },
                        function (from, to) {

                        }
                    ],
                    decks: [
                        // Destine deck is empty
                        // Different Color
                        function (from, to) {
                            return from.card.suit.color != to.card.suit.color
                        },
                        // Origin card is not Flipped
                        function (from, to) {
                            return ! from.card.flipped
                        },
                        // Destine card is not Flipped
                        function (from, to) {
                            return ! to.card.flipped
                        },
                        // Origin card is lower than Destine
                        function (from, to) {
                            return from.card.entity.number < to.card.entity.number
                        }
                    ]
                },
                cards: {
                    entities: [{rep:'A', number:1}, {rep:'2', number:2}, {rep:'3', number:3}, {rep:'4', number:4}, {rep:'5', number:5}, {rep:'6', number:6}, {rep:'7', number:7}, {rep:'8', number:8}, {rep:'9', number:9}, {rep:'10', number:10}, {rep:'J', number:11}, {rep:'Q', number:12}, {rep:'K', number:13}],
                    suits: [{rep: '♠', color: 'black', name: 'spades'}, {rep: '♥', color: 'red', name: 'hearts'}, {rep: '♦', color: 'red', name: 'diams'}, {rep: '♣', color: 'black', name: 'clubs'}],
                },
                spots: {
                    rules: {
                        begins: [0,0,0,0,1,2,3,4,5,6,7,8,16],
                        beginsUnflipped: [0,0,0,0,1,1,1,1,1,1,1,1,0],
                        movement: ['suitDecks','suitDecks','suitDecks','suitDecks','decks','decks','decks','decks','decks','decks','decks','decks','well']
                    },
                    spots: {
                        1:[], 2:[], 3:[], 4:[], 5:[], 6:[], 7:[], 8:[], 9:[], 10:[], 11:[], 12:[], 13:[]
                    },
                    scores: {
                        moves: 0,
                        score: 0,
                        playing: false
                    }
                }
            },
            session: {
                spots: { 1:[], 2:[], 3:[], 4:[], 5:[], 6:[], 7:[], 8:[], 9:[], 10:[], 11:[], 12:[], 13:[] },
                scores: {
                    moves: 0,
                    score: 0,
                },
                playing: false
            }
        }
    },
    computed: {
        // implement to know if is possible a next action..
        hasNextMove: function () {
            return (this.session.playing && false)
        }
      },
      methods: {
        // Called when user try to move a card.
        tryMoveCard: function () {

        },
        // Try Apply rules to catch if movement is possible
        isPossibleMovement: function (from, to) {

        },
        // Move cards from a spot to another
        makeMovement: function (from, to) {

        },
        // Returns a object with from,to with card and deck infos.
        generateMovementObj: function () {

        },
        // Add or Remove score points, based on movement.
        updateScore: function (amount) {
            this.session.scores.score += amount
        },
        // Increment Move Counter
        touchMovesCounter: function () {
            this.session.scores.moves += 1
        },
        // Generate full deck of cards (4 suits)
        generateCards: function () {
            var cards = []
            var that = this
            this.defaults.cards.suits.forEach(function(s) {
                that.defaults.cards.entities.forEach(function(e) {
                    cards.push({
                        suit: s,
                        entity: e,
                        flipped: true
                    });
                })
            })
            return cards
        },
        // Called when click on new game button
        newGame: function () {
            this.resetSpots()
            var cards = this.generateCards()
            cards = this.randomizeCards(cards) // could be a nice idea have a pipe method to call.
            this.applyNewGame(cards)
            this.unflipCardsFromSpots()
        },
        // Set all spots to blank
        resetSpots: function () {
            this.session.spots =  {1:[], 2:[], 3:[], 4:[], 5:[], 6:[], 7:[], 8:[], 9:[], 10:[], 11:[], 12:[], 13:[]}
        },
        // Put cards into spot for new game
        applyNewGame: function (cards) {
        var that = this
        this.defaults.spots.rules.begins.forEach(function(amount, index) {
            var realIndex = index + 1
            for(var y = 1; y <= amount; y++){
                that.session.spots[realIndex].push(cards.pop())
            }
        })
        },
        // Sorts a deck with (4 suits)
        randomizeCards: function (cards) {
            for(var x = 1; x < 100; x++){
                cards.sort(function(){
                    return .5 - Math.random()
                })
            }
            return cards
        },
        // Unflip cards from decks to start game.
        unflipCardsFromSpots: function () {
            var that = this
            this.defaults.spots.rules.beginsUnflipped.forEach(function(item, key){
                var index = key+1
                for (var w = 1; w <= item; w++){
                    that.session.spots[index][w-1].flipped = false
                }
            })
        }
      }
    }
</script>

<style>
    body{
        background-image:url(http://www.photoshopbuzz.com/wp-content/uploads/2013/06/woodtexture5.jpg);
    }

    .spots-row{
        margin: 0;
        padding: 0;
        list-style-type: none;
    }

    ul.spots-row li.spot-container{
        display: inline-block;
    }

    #well {
        position:absolute;
        top: 10px;
        left: 600px;
    }

    #decks {
        position:absolute;
        top:300px;
        left: 10px;
    }

    #suit-decks {
        position:absolute;
        top:10px;
        left:10px;
    }
</style>
