<template>

<div class="game-area">
    <h1 class="title">Guess the <span>Cat!</span></h1>
    <h4 class="description">Select one of the 6 card, then click the closed card.</h4>
    <div class="container">
        <transition-group name="rotate-all" class="card-container" appear>
            <app-card 
                    v-for="card in cards" 
                    :key="card.id"
                    :card="card"
                    @click.native="selectedCard = card.id"
                    :class="{'shadow': selectedCard == card.id}">
            </app-card>
        </transition-group>
    </div>

    <div class="container">
        <transition name="rotate" mode="out-in">
            <component 
            :is="activeCard"
            @click.native="showCard(answer)"
            :card="answer">

            </component>
        </transition>
    </div>
</div>

</template>

<script>

import CardVue from "./Card.vue";
import DefaultCardVue from "./DefaultCard.vue"

export default {
    components: {
        appCard : CardVue,
        appDefaultCard: DefaultCardVue,
    },
    data(){
        return {
            selectedCard: null,
            answer: {},
            activeCard: "appDefaultCard",
            cards: [
                {id: 1, component: "app-card", image: "/src/assets/cat1.jpg"},
                {id: 2, component: "app-card", image: "/src/assets/cat2.jpg"},
                {id: 3, component: "app-card", image: "/src/assets/cat3.jpg"},
                {id: 4, component: "app-card", image: "/src/assets/cat4.jpg"},
                {id: 5, component: "app-card", image: "/src/assets/cat5.jpg"},
                {id: 6, component: "app-card", image: "/src/assets/cat6.jpg"}
            ]
        }
    },
    created(){
        let randomCard = Math.ceil(Math.random()*this.cards.length);
        this.answer = this.cards[randomCard - 1];
    },
    methods: {
        showCard(answer){

            if(this.selectedCard == null) {
                alert("Select a card.");
            }
            else {
                this.activeCard = answer.component;
                setTimeout(() => {
                    if (answer.id == this.selectedCard) {
                        this.$emit("activeComponentEvent", "app-celebrate");
                    }
                    else{
                        this.$emit("activeComponentEvent", "app-failure");
                    }
                },800);
            }
            

            
        }
    }
}


</script>

<style scoped>

.title {
    text-align: center;
    color: rosybrown;
}

.title span {
    color: mediumvioletred;
}

.description {
    color: grey;
    text-align: center;
}

.container, .card-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 50px;
}

.shadow {
    box-shadow: 0px 5px 48px #30969f !important;
    transition: box-shadow .5s;
}


/* Opened Card Animations */

/* .rotate-all-enter{
    
} */
.rotate-all-enter-active{
    animation:rotate-all ease-in-out 2s forwards;
}
/* .rotate-all-leave{} */
/* .rotate-all-leave-active{} */


@keyframes rotate-all {
    from {
        transform: rotateY(0);
    }
    to {
        transform: rotateY(1080deg);
    }
}

/* Closed Card Animation */

/* .rotate-enter {} */
.rotate-enter-active{
    animation: rotate-in .3s ease-in-out forwards;
}
/* .rotate-leave {} */
.rotate-leave-active{
    animation: rotate-out .3s ease-in-out forwards;
}

@keyframes rotate-in {
    from {
        transform: rotateY(90deg);
    }
    to{
        transform: rotateY(0deg);
    }
}

@keyframes rotate-out {
    from {
        transform: rotateY(0deg);
    }
    to {
        transform: rotateY(90deg);
    }
}


</style>
