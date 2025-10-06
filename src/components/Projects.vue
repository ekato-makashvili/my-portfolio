<template>
  <section id="projects" class="py-20 max-w-6xl mx-auto text-center">
    <h2 class="text-4xl font-bold mb-12">Projects</h2>

    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-10">
      <div
        v-for="(proj, i) in projects"
        :key="i"
        @click="openProject(proj.link)"
        class="relative bg-white/20 rounded-2xl shadow-lg overflow-hidden aspect-[2] hover:scale-105 hover:-rotate-1 transition-all duration-300 cursor-pointer"
      >
        <img :src="proj.img" :alt="proj.title" class="w-full h-full object-cover" />
        <div class="absolute inset-0 bg-black/40 opacity-0 hover:opacity-100 flex items-center justify-center transition-all duration-300">
          <p class="text-xl font-semibold">View Project →</p>
        </div>
        <div class="absolute bottom-0 w-full bg-black/50 p-3">
          <h3 class="text-lg font-bold">{{ proj.title }}</h3>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed } from "vue";

const projects = computed(() => [
  {
    img: new URL("@/assets/projects/solar-system.jpg", import.meta.url).href,
    title: "3D Galaxy Solar System",
    link: "https://3d-galaxy-solar-system.netlify.app/",
  },
  {
    img: new URL("@/assets/projects/portfolio.jpg", import.meta.url).href,
    title: "Compare Texts",
    link: "https://text-diff-checker.netlify.app/",
  },
  {
    img: new URL("@/assets/projects/jewelry.jpg", import.meta.url).href,
    title: "Jewelry Store",
    link: "https://zvirti-jewelry-store.netlify.app/",
  },
]);

function openProject(link) {
  window.open(link, "_blank");
}
</script>
