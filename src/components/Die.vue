<template>
<div :class="{die: true , isHeld: this.isHeld}" v-on:click="toggleDice">
    <div class="die-number">{{ value }}</div>
</div>
</template>

<script>
export default {
    name: "Die",
    props: {
        value : Number,
        isHeld: Boolean,
        toggleDice : Function
    }
}
</script>
