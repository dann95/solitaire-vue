<template lang="html">
    <div>
        <deck></deck>
        <suit-deck></suit-deck>
        <well-deck></well-deck>
    </div>
</template>

<script type="text/javascript">
    import Deck from './components/Deck'
    import SuitDeck from './components/SuitDeck'
    import WellDeck from './components/WellDeck'
    export default {
        name: 'app',
        components: {
            Deck,
            SuitDeck,
            WellDeck
        },
        mounted(){
            /* Event Listener */
            var that = this
            window.Events.$on('', function () {

            })
        },
        data(){
            return {
                /* Session */
                session: {

                },
                /* History */
                history: {

                }
            }
        },
        methods: {
            /* Rules */
            rules: function () {

            },
            getRulesForSpot: function (number) {

            },
            isPossibleMovement: function (movement) {

            },
            /* Movements */
            tryMoveCard: function (movement) {

            },
            makeMovement: function (movement) {

            },
            movementRoutine: function (movement) {

            },
            identifyMovement: function (movement) {

            },
            /* Score and Time */
            touchMovesCounter: function () {

            },
            updateScore: function (amount) {

            },
            startCounter: function () {

            },
            stopCounter: function () {

            },
            updateBonusRate: function (amount) {

            },
            /* Cards */
            generateCards: function () {

            },
            randomizeCards: function (cards) {

            },
            unflipLastCardFromSpot: function (number) {

            },
            unflipRoutine: function () {

            },
            /* Game Controls */
            onNewGameClick: function () {

            },
            /*  */
            startGame: function () {

            },
            restartGame: function () {

            },
            onHelpClick: function () {

            },
            /* Game Effects */
            onGameStart: function () {

            },
            onGameFinished: function (winner = false) {

            },
            /* Defaults */
            defaults: function () {
                return {

                }
            },
            /* Spots */
            resetSpots: function () {

            }
        },
        computed: {

        }
    }
</script>

<style lang="css">
</style>
