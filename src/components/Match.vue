<template>
    <div class="match">
        <div class="match-infos">
            <div class="team_names">
                <p class="blue_name">{{ blue_team_name }}</p>
                <p class="orange_name">{{ orange_team_name }}</p>
            </div>
            <div class="team_scores">
                <p :class="blue_team_score > orange_team_score ? 'winner' : 'looser'">{{ blue_team_score }}</p>
                <p :class="blue_team_score < orange_team_score ? 'winner' : 'looser'">{{ orange_team_score }}</p>
            </div>
        </div>
        <div class="match-sub-infos">
            <p class="event">{{ match_event }}</p>
            <p class="date">{{ match_date_infos.date }} - {{ match_date_infos.time }}</p>
        </div>
    </div>
</template>

<script setup>
const props = defineProps(['match']);

const orange_team_name = props.match.orange.team.team.name;
const blue_team_name = props.match.blue.team.team.name;

const blue_team_score = props.match.blue.score ? props.match.blue.score : 0;
const orange_team_score = props.match.orange.score ? props.match.orange.score : 0;

const match_event = props.match.event.name;

const match_full_date = props.match.date;

const match_time = match_full_date.split('T')[1].split('Z')[0];
const match_date = match_full_date.split('T')[0];

const match_date_infos = {
    date: match_date,
    time: match_time
};

</script>