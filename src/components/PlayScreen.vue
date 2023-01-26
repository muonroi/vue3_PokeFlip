<template>
    <div class="screen">
        <div class="screen__inner"
        :style="{
        width: `${
          ((((720 - 16 * 4) / Math.sqrt(flipElements.length) - 16) * 3) / 4 +
            16) *
            Math.sqrt(flipElements.length)
        }px`,
        }"
        >
            <card-component v-for = "(element,index) in flipElements" 
            :key="index"
            ref="card"
    :cardImg="`images/${element}.png`"
    :cardSelected="{index: index,value:element}"
    :totalElement="flipElements"
    @sendCardFlipped="CardSelected"
    :isRule="selectedCards"
    />
    </div>
    </div>
</template> 
<script>
import CardComponent from "./CardFlip.vue";
export default {
    
    props:{
        flipElements:{
            type:Array,
            default: function(){
                return []
            }
        }
    },
    components:{
        CardComponent,
    },
    data(){
return {
    selectedCards:[],
    cardSelectTotal:[],
}
    },
    methods:{
        CardSelected(card) {
            console.log(this.selectedCards.length);
            if(this.selectedCards.length === 2) return false;
            this.selectedCards.push(card);
            if(this.selectedCards.length === 2)
            {
                const checkCards = this.selectedCards[0].value === this.selectedCards[1].value;
                if(!checkCards)
                {
                    setTimeout(() =>
                    {
                        this.$refs.card[this.selectedCards[0].index].onFlipBackCard();
                        this.$refs.card[this.selectedCards[1].index].onFlipBackCard();
                        this.selectedCards = [];
                    },800)
                }
                if(checkCards)
                {
                    setTimeout(() =>
                    {
                        this.$refs.card[this.selectedCards[0].index].setStatusCard();
                        this.$refs.card[this.selectedCards[1].index].setStatusCard();
                        this.cardSelectTotal.push(card)
                        this.selectedCards = [];
                    },800);
                }
                else
                {
                    return false;
                }
                
            }
            if(this.cardSelectTotal.length === ((this.flipElements.length)/2 - 1))
            {
                this.$emit("onPauseGame",true);
            }
    }
}
}
</script>
<style lang="css" scoped>
.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light);
}
.screen__inner {
    width: calc(424px);
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
}
</style>