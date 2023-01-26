<template>
            <div class="card" :class="{'disabled':isFlipped}"
            :style="{
      height: `${(720 - 16 * 4) / Math.sqrt(totalElement.length) - 16}px`,
    width: `${
        (((720 - 16 * 4) / Math.sqrt(totalElement.length) - 16) * 3) / 4
    }px`,
    perspective: `${
        ((((720 - 16 * 4) / Math.sqrt(totalElement.length) - 16) * 3) / 4) * 2
    }px`,
    }"
            >
                <div class="card__inner" @click="ChangeFace" :class ="{'is-flipped':isFlipped}">
                    <div class="card__face card__face--front">
                        <div class="card__content" :style="{
            'background-size': `${
              (((720 - 16 * 4) / Math.sqrt(totalElement.length) - 16) * 3) /
            4 /
            3
            }px ${
              (((720 - 16 * 4) / Math.sqrt(totalElement.length) - 16) * 3) /
            4 /
            3
            }px`,
        }"></div>
                    </div>
                    <div class="card__face card__face--back">
                        <div class="card__content" :style="
                        {
                            backgroundImage: `url('${require('@/assets/' + cardImg)}')`,
                        }">
                        </div>   
                    </div>
                </div>
            </div>
</template>
<script>
export default{
    props:{
        cardImg:{
            type: String,
            required:true
        },
        cardSelected:{
            type: [Object,String,Array,Number],
            default: () => []
        },
        totalElement:{
            type: Array,
            default: () => []
        },
        isRule:{
            type:Array,
            default: () => []
        }
    },
    data() {
        return{
            isFlipped:false,
            isDisable:false

        }
    },
    methods:{
        setStatusCard()
        {
            this.isDisable = true
        },
        ChangeFace(){
            if(this.isRule.length >= 2) return false;
            if(this.isDisable) return false;
            this.isFlipped = !this.isFlipped;
            if(this.isFlipped) this.$emit('sendCardFlipped',this.cardSelected)
        },
        onFlipBackCard()
        {
            this.isFlipped = false;
        },
    }
    
}
</script>
<style scoped>

.card{
    display:inline-block;
        margin-right: 1rem;
        margin-bottom: 1rem;
    }
.card__inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
    background-color: var(--dark);
}
.card.disabled .card__inner {
    cursor: default;
}

.card__inner.is-flipped {
    transform: rotateY(-180deg);
}

.card__face {
    width: 100%;
    position: absolute;
    backface-visibility: hidden;
    height: 100%;
    border-radius: 1rem;
    overflow: hidden;
    box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
    padding: 1rem;
}

.card__face--front .card__content {
    height: 100%;
    background: url("../assets/images/icon_back.png") no-repeat center center;
    background-size: 60px 60px;
    width: 100%;
}

.card__face--back {
    transform: rotateY(-180deg);
    background-color: var(--light);
}

.card__face--back .card__content {
    background-repeat: no-repeat;
    background-position: center center;
    height: 100%;
    background-size: contain;
    width: 100%;
}
</style>