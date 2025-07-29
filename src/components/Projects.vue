<template>
  <!-- My Projects Section -->
  <section class="pb-5" id="projects">
    <!-- Section Heading -->
    <h1 class="mt-5 mb-5 pb-4 text-center">My Projects</h1>

    <!-- Loop through each group of 3 projects -->
    <div
      v-for="(group, index) in chunkedProjects"
      :key="index"
      class="card-deck my-5 justify-content-center"
    >
      <!-- For each project in the current group, render a ProjectCard -->
      <ProjectCard
        v-for="project in group"
        :key="project.id"
        :project="project"
      />
    </div>
  </section>
</template>

<script setup>

import { computed, ref } from 'vue';


import ProjectCard from './ProjectCards.vue';


import projectsRaw from '../data/projects.json';


const projects = ref(projectsRaw);

const chunkSize = 3;


const chunkedProjects = computed(() => {
  const chunks = [];
  for (let i = 0; i < projects.value.length; i += chunkSize) {
    chunks.push(projects.value.slice(i, i + chunkSize));
  }
  return chunks;
});
</script>
