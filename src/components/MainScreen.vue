<template>
    <div class="main__screen">
        <h1 class="title__game">POKE FLIP</h1>
        <p class = "select__game--mode">Select Mode To Start Game</p>
        <div class="mode">
            <button @click="GetTotalSelect(16)">4x4</button>
            <button @click="GetTotalSelect(36)">6x6</button>
            <button @click="GetTotalSelect(64)">8x8</button>
        </div>
    </div>
</template>
<script>
//import {ref} from 'vue';
export default{
    methods:{
        GetTotalSelect(total){
            this.$emit("GetTotalSelect",{total});
        }
    }
}
</script>
<style scoped>
.main__screen{
    background-color: var(--dark);
    width:100%;
    height: 100vh;
    top:0;
    left:0;
    position: absolute;
    z-index:2;
    display:flex;
    justify-content: center;
    flex-direction: column;
    color: var(--light);
    align-items: center;
}
.main__screen .title__game{
    font-size: 4.5rem;
    text-transform: uppercase;
}
.main__screen .select__game--mode{
    font-size: 2rem;
}
.mode{
    margin-top: 2rem;
    display: flex;
}
.mode button{
    width:150px;
    height:150px;
    margin:0 1rem;
    border-radius: 1rem;
    font: var(--font);
    color:var(--light);
    background: transparent;
    border: 1px solid var(--light);
    box-shadow: none;
    cursor: pointer;
    font-size: 2rem;
}
.mode button:hover{
    background:rgba(0, 0,0, .2)
}
</style>
