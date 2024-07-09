<template>
    <div class="container matches">
        <h1>{{ paginatedMatchList.length === 0 ? 'No match found' : 'Found matches' }}</h1>
        <ul>
            <li class="match-item" v-for="match in paginatedMatchList" :key="match._id">
                <Match :match="match" />
            </li>
        </ul>
        <div class="pages" v-if="paginatedMatchList.length > 0">
            <button @click="HandlePreviousPage">Previous</button>
            <p>{{ current_page.value }}</p>
            <button @click="HandleNextPage">Next</button>
        </div>
    </div>
</template>

<script setup>
import Match from './Match.vue';
import { reactive, onMounted, watch, computed } from 'vue';

const props = defineProps(['searchParameters']);

const matchList = reactive([]);
const current_page = reactive({ value: 1 });
const batch_size = 200; // Number of matches to fetch
const matchesPerPage = 15; // Number of matches per page
const start_date = '2024-01-01'; // Start date for fetching matches

const HandlePreviousPage = () => {
    if (current_page.value > 1) {
        current_page.value -= 1;
    }
    console.log('Current page:', current_page.value);
};

const HandleNextPage = () => {
    if (current_page.value * matchesPerPage < filteredMatchList.value.length) {
        current_page.value += 1;
    }
    console.log('Current page:', current_page.value);
};

// Function to fetch matches
const fetchMatches = async () => {
    try {
        console.log('Fetching matches...');

        const url = `https://zsr.octane.gg/matches?after=${start_date}&perPage=${batch_size}`;
        const response = await fetch(url);

        if (!response.ok) {
            console.error('Failed to fetch matches');
            return;
        }

        const data = await response.json();

        matchList.length = 0; // Clear existing matches
        matchList.push(...data.matches);

        console.log('Fetched matches', matchList);
    } catch (error) {
        console.error('Error fetching matches', error);
    }
};

// Fetch matches when the component is mounted
onMounted(fetchMatches);

// Watch for changes in searchParameters prop
watch(() => props.searchParameters, (newParameters) => {
    console.log('Search parameters changed:', newParameters);
    filterMatches();
}, { deep: true });

// Function to filter matches
const filterMatches = () => {
    current_page.value = 1; // Reset to first page when filtering
};

// Computed property for filtering matches
const filteredMatchList = computed(() => {
    const parameters_team = props.searchParameters.team ? props.searchParameters.team : '';
    const parameters_event = props.searchParameters.event ? props.searchParameters.event : '';
    const parameters_date = props.searchParameters.date ? props.searchParameters.date : '';

    console.log('Filtering matches containing:', parameters_team, parameters_event, parameters_date);
    return matchList.filter(match => {
        const matchesTeam = match.blue.team.team.name.toLowerCase().includes(parameters_team.toLowerCase()) || match.orange.team.team.name.toLowerCase().includes(parameters_team.toLowerCase());
        const matchesEvent = match.event.name.toLowerCase().includes(parameters_event.toLowerCase());
        const matchesDate = match.date.includes(parameters_date);
        return matchesTeam && matchesEvent && matchesDate;
    });
});

// Computed property for paginated matches
const paginatedMatchList = computed(() => {
    const start = (current_page.value - 1) * matchesPerPage;
    const end = start + matchesPerPage;
    return filteredMatchList.value.slice(start, end);
});

</script>