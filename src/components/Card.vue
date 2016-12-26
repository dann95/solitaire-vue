<template>
    <div>
        <div class="card back" v-if="instance.flipped">*</div>
        <div class="card rank-7" :class="instance.suit.name" v-else>
            <span class="rank" v-text="instance.entity.rep"></span>
            <span class="suit" v-text="instance.suit.rep"></span>
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return{

            }
        },
        methods: {
          handleDrop: function() {

          }
        },
        props:['instance']
    }
</script>
