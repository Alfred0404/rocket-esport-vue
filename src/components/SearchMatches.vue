<template>
    <div class="container search-matches">
        <p>Search for matches the easy way</p>
        <form onsubmit="return false">
            <div class="form-line team">
                <label>Team</label>
                <input type="text" placeholder="Select the team" v-model="team" />
            </div>
            <div class="form-line event">
                <label>Event</label>
                <input type="text" placeholder="Select the event" v-model="event" />
            </div>
            <div class="form-line date">
                <label>Date</label>
                <input type="date" placeholder="Select the date" v-model="date" />
            </div>
            <button type="submit" @click="onSubmit">Submit</button>
            <button type="submit" @click="onUpcomingMatchesSubmit">Upcoming matches</button>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue';

// events to filter matches
const emit = defineEmits(['MatchesSearchParameters', 'searchUpcomingMatches']);

const team = ref('');
const event = ref('');
const date = ref('');

const current_date = new Date().toISOString().split('T')[0];
console.log('Current date : ', current_date);

const onSubmit = () => {
    console.log('Search parameters from the form : ', team.value, event.value, date.value);
    emit('MatchesSearchParameters',
    {
        team: team.value,
        event: event.value,
        date: date.value
    }
    );
};

const onUpcomingMatchesSubmit = () => {
    console.log('Search for upcoming matches');
    emit('searchUpcomingMatches',
    {
        date: current_date
    }
    );
};

</script>