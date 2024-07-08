<template>
    <main class="main">
        <h1 v-if="tenzies" class="main-title won">You Won</h1>
        <h1 v-else class="main-title">Tenzies</h1>

        <p class="main-info"> Roll until all dice are the same. Click each die to freeze it at its
            current value between rolls. </p>

        <div class="die-container">
            <Die v-for="die in this.dice" :key="die.id" :value="die.value" :isHeld="die.isHeld"
                :toggleDice="() => toggleHeld(die.id)" />
        </div>

        <button v-if="tenzies" class="main-btn" v-on:click="newGame">New Game</button>
        <button v-else class="main-btn" v-on:click="rollDice">Roll</button>
    </main>
</template>

<script>
import Die from './components/Die.vue';

export default {
    name: "App",
    components: {
        Die
    },
    data() {
        return {
            dice: this.allNewDice(),
            tenzies : false
        }
    },
    methods: {

        allNewDice() {
            const newDice = [];
            for (let i = 0; i < 10; i++) {
                const currentObj = {
                    id: i + 1,
                    value: Math.ceil(Math.random() * 6),
                    isHeld: false
                }
                newDice.push(currentObj)
            }
            return newDice
        },

        rollDice() {
            const newDice = []
            for (let i = 0; i < this.dice.length; i++) {
                const currentDice = this.dice[i]
                if (currentDice.isHeld === false) {
                    const updateDice = {
                        ...currentDice,
                        value: Math.ceil(Math.random() * 6)
                    }
                    newDice.push(updateDice)
                } else {
                    newDice.push(currentDice)
                }
            }
            this.dice = newDice
        },

        toggleHeld(id) {
            const newDice = []
            for (let i = 0; i < this.dice.length; i++) {
                const currentObj = this.dice[i]
                if (currentObj.id === id) {
                    const updateHeld = {
                        ...currentObj,
                        isHeld: !currentObj.isHeld
                    }
                    newDice.push(updateHeld)
                } else {
                    newDice.push(currentObj)
                }
            }
            this.dice = newDice
        }, 

        newGame(){
            this.tenzies = false 
            this.dice = this.allNewDice()
        }
    },
    updated(){
        const allHeld = this.dice.every( die => die.isHeld);
        const firstValue = this.dice[0].value;
        const sameValue = this.dice.every((die) => die.value === firstValue);
        if (allHeld && sameValue){
            this.tenzies = true;
        }
    }
}
</script>
