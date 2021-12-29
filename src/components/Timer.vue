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
	<button class="keypad__start-button" 
		@click="startTimer" 
		:disabled="isStartDisabled"
	>
		Start
	</button>

	</div>
</template>

<script>
export default {
	name: 'Timer',
	data() {
		return {
			timerIntegerButtons: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
			timerValues: [0, 0, 0, 0],
			timerCountdown: null
		}
	},
	computed: {
		minutes() {
			return `${this.timerValues[0]}${this.timerValues[1]}`
		},
		seconds() {
			return `${this.timerValues[2]}${this.timerValues[3]}`
		},
		timerInMiliseconds() {
			const minutes = this.timerValues[0] * 10 + this.timerValues[1]
			const seconds = this.timerValues[2] * 10 + this.timerValues[3]
			return minutes * 60000 + seconds * 1000
		},
		isStartDisabled() {
			return this.timerInMiliseconds === 0 || this.timerCountdown
		}
	},
	methods: {
		updateTimer(valueToAdd) {
			const newTimer = [...this.timerValues]
			newTimer.push(valueToAdd)
			this.timerValues.push(valueToAdd)
			this.timerValues.shift()
		},
		startTimer() {
			this.timerCountdown = setInterval(this.subtractOneSecond, 1000)
		},
		subtractOneSecond() {
			const [tenMins, oneMins, tenSec, oneSec] = this.timerValues
			if (oneSec > 0) {
				this.timerValues = [tenMins, oneMins, tenSec, oneSec - 1]
			} else if (tenSec > 0) {
				this.timerValues = [tenMins, oneMins, tenSec - 1, 9]
			} else if (oneMins > 0) {
				this.timerValues = [tenMins, oneMins - 1, 5, 9]
			} else if (tenMins > 0) {
				this.timerValues = [tenMins - 1, 9, 5, 9]
			} else {
				this.stopTimer()
			}
		},
		stopTimer() {
			clearInterval(this.timerCountdown)
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
