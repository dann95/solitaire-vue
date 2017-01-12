<template lang="html">

</template>

<script type="text/javascript">
export default {
    name: 'card',
    data(){
        return {
            suits: {
                spades: {},
                hearts: {},
                diams: {},
                clubs: {}
            }
        }
    },
    props: ['suit','rep','flipped']
}
</script>

<style lang="css">
</style>
