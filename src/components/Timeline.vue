<script setup lang="ts">
import { DateTime } from "luxon";
import { ref, computed } from "vue";
import TimelineItem from "./TimelineItem.vue"; 
import { usePosts } from "../stores/posts";
import { Period, periods } from "../constants";
import { TimelinePost, today, thisWeek, thisMonth } from "../posts";

const postStore = usePosts();

await postStore.fetchPosts();

const selectedPeriod = ref<Period>("Today");

function selectPeriod(period: Period) {
    console.log(period);
    selectedPeriod.value = period;
}




</script>

<template>
    <nav class="is-primary panel">
        <span class="panel-tabs">
            <a v-for="period of periods"
            :key="period"
            @:click="postStore.setSelectedPeriod(period)"
            >
                {{ period }}
            </a>
        </span>
        <TimelineItem 
            v-for="post of postStore.filteredPosts"
            :key="post.id" 
            :post="post"
            />
    </nav>

</template>

<style>

</style>