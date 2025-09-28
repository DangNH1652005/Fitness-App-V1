<script setup>
    import Grid from '../Grid.vue'
    import { gymHealthFacts } from '../../utils'

    const props = defineProps({
        handleSelectWorkout: Function,
        firstInCompleteWorkoutIndex: Number,
        handleResetPlan: Function,
    })

    const randomNumber = Math.floor(Math.random() * gymHealthFacts.length)
    const todaysFact = gymHealthFacts[randomNumber]

</script>

<template>
    <section id="dashboard">
        <div class="card tip-container">
            <h2>Welcome Spidey</h2>
            <div>
                <p class="tip"><strong>Daily Tip</strong><br/>{{ todaysFact }}</p>
            </div>
            <button @click="() => handleSelectWorkout(firstInCompleteWorkoutIndex < 0 ? 0 : firstInCompleteWorkoutIndex)">Start workout &rarr;</button>
        </div>
        <Grid v-bind="props" />
    </section>
</template>

<style scoped>
    .tip-container,
    .tip-container div,
    #dashboard {
        display: flex;
    }

    .tip-container,
    #dashboard {
        flex-direction: column;
    }

    #dashboard {
        gap: 2rem;
    }

    .tip-container {
        gap: 0.5rem;
    }

    @media (min-width: 640px) {
        .tip-container {
            gap: 1rem;
        }
    }
</style>
