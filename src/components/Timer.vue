<template lang="ts">
	<div class="timer-wrapper">
	{{minutes}}:{{seconds}}
	<div class="keypad">
	<button class="keypad__button" 
		v-for="(integer, index) in timerIntegerButtons" 
		:key="integer" 
		@click="updateTimer(integer)"
	>
	{{ integer }}
	</button>
	</div>

	</div>
</template>

<script>
export default {
	name: 'Timer',
	data() {
		return {
			timerIntegerButtons: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
			timerValues: [0, 0, 0, 0],
		}
	},
	computed: {
		minutes() {
			return `${this.timerValues[0]}${this.timerValues[1]}`
		},
		seconds() {
			return `${this.timerValues[2]}${this.timerValues[3]}`
		},
	},
	methods: {
		updateTimer(valueToAdd) {
			const newTimer = [...this.timerValues]
			newTimer.push(valueToAdd)
			this.timerValues.push(valueToAdd)
			this.timerValues.shift()
		},
	}
}
</script>

