<template>
    <div id="solitaire" class="playingCards twoColours rotateHand">
        <div id="suit-decks">
            <div class="container">
                <ul class="spots-row">
                    <li class="spot-container">
                        <spot :cards="session.spots[1]" number="1"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[2]" number="2"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[3]" number="3"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[4]" number="4"></spot>
                    </li>
                </ul>
                <div id="well">
                    <spot :cards="session.spots[13]" number="13"></spot>
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
                        <spot :cards="session.spots[5]" number="5"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[6]" number="6"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[7]" number="7"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[8]" number="8"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[9]" number="9"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[10]" number="10"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[11]" number="11"></spot>
                    </li>
                    <li class="spot-container">
                        <spot :cards="session.spots[12]" number="12"></spot>
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
    mounted: function () {
        // bind this to private scope
        var that = this
        // when Any spot broadcasts a movement, we catch it.
        window.Event.$on('movement', function (movement) {
            var move = that.injectSpotsToMovement(movement)
            that.tryMoveCard(move)
        })
    },
    data: function () {
        return {
            defaults: {
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
        tryMoveCard: function (movement) {
          // check if is valid movement
          if(this.isPossibleMovement(movement)){
            // apply movement
            this.makeMovement(movement)
          }
        },
        // Try Apply rules to catch if movement is possible
        isPossibleMovement: function (movement) {
          var rules = this.getRulesForSpot(movement.to.deck.number)
          rules = rules.map(function(r){ return r(movement) })
          return ! (rules.indexOf(false) > -1)
        },
        // get rules array of functions to specific Destination spot.
        getRulesForSpot: function (number) {
            return this.rules()[this.defaults.spots.rules.movement[number]]
        },
        // get rules for game.
        rules: function () {
          return {
              well: [
                  function (movement) {
                      return false
                  }
              ],
              suitDecks: [],
              decks: [
                  // Destine deck is empty or Destine Card is not Flipped
                  function (movement) {
                      return (movement.to.deck.bag.length == 0) || (movement.to.deck.bag[0].flipped == false)
                  },
                  // Different Color
                  function (movement) {
                      return movement.to.deck.bag[0].suit.color != movement.card.suit.color
                  },
                  // Origin card is not Flipped
                  function (movement) {
                      return movement.card.flipped != true
                  },
                  // Origin card is lower than Destine
                  function (movement) {
                      return (movement.to.deck.bag.length == 0) || ( (movement.card.entity.number+1) == movement.to.deck.bag[0].entity.number)
                  }
              ]
          }
        },
        // Move cards from a spot to another
        makeMovement: function (movement) {
          //@TODO know if card is top from stack, if not know if below cards are sequence, and move all.
          // make movement
          this.session.spots[movement.to.deck.number].unshift(movement.card)
          this.session.spots[movement.from.deck.number].splice(0,1)
          // increase movement Counter
          this.touchMovesCounter()
          // increase score
          this.updateScore(100)

          // Apply routine after each movement
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
        },
        // add From and To spot arrays for apply rules.
        injectSpotsToMovement: function (m) {
            var from = m.from
            var to = m.to
            return {
                from: {
                  deck: {
                    bag: this.session.spots[from],
                    number: from
                  }
                },
                to: {
                  deck: {
                    bag: this.session.spots[to],
                    number: to
                  }
                },
                card: m.card
            }
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
