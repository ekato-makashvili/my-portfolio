<template>
  <div class="relative min-h-screen overflow-hidden" @mousemove="onMouseMove">
    <!-- Gradient background -->
    <div
      class="absolute inset-0 animate-gradient bg-gradient-to-r from-cyan-700 via-blue-500 to-cyan-400 z-0"
    ></div>

    <!-- Floating particles -->
    <div
      v-for="(particle, i) in particles"
      :key="i"
      class="absolute rounded-full opacity-30 bg-white z-10"
      :style="particle.style"
    ></div>

    <!-- Main content -->
    <div class="relative z-20">
      <!-- Header Section -->
      <header class="relative text-center fade-up mt-10">
        <button
          @click="toggleLanguage"
          class="absolute right-6 bg-white text-cyan-800 font-semibold px-4 py-2 rounded-full shadow-lg hover:scale-110 hover:bg-cyan-100 transition-transform duration-300"
        >
          🌐 {{ currentLang === "ka" ? "EN" : "KA" }}
        </button>

        <h1 class="text-5xl font-bold mb-4">{{ texts.name }}</h1>
        <p class="text-xl mb-2">{{ texts.profession }}</p>
        <p class="mt-2">{{ texts.contact }}</p>
      </header>

      <!-- Skills -->
      <div
        class="skills-container flex flex-wrap justify-center gap-6 relative z-10"
      >
        <div
          v-for="(skill, i) in skills"
          :key="i"
          class="skill-box"
          :style="{ transform: `translateY(${waveY[i] + 50}px)` }"
        >
          {{ skill }}
        </div>
      </div>

      <!-- Wave Separator -->
      <svg
        class="wave-separator w-full"
        viewBox="0 0 1440 150"
        preserveAspectRatio="none"
      >
        <path
          ref="wavePath"
          fill="#0ea5e9"
          d="M0,30 C360,90 1080,-30 1440,30 L1440,150 L0,150 Z"
        ></path>
      </svg>

      <!-- Projects Section -->
      <section class="-mt-[60vh]">
        <h2 class="text-3xl font-semibold text-center mb-8">
          {{ texts.projectsTitle }}
        </h2>
        <div class="grid md:grid-cols-3 gap-8 max-w-6xl mx-auto px-6">
          <div
            v-for="(proj, i) in projects"
            :key="i"
            class="bg-white rounded-2xl shadow-lg overflow-hidden transform hover:scale-105 hover:-rotate-1 transition-all duration-300 fade-up"
            :style="{ transitionDelay: `${i * 150}ms` }"
          >
            <img :src="proj.img" :alt="proj.title" class="w-full" />
            <div class="p-4 text-center">
              <h3 class="text-xl font-bold mb-2">{{ proj.title }}</h3>
              <button
                class="bg-white text-cyan-800 font-semibold px-4 py-2 rounded-xl hover:bg-cyan-100 hover:scale-110 transition-transform duration-300"
              >
                {{ texts.more }}
              </button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

/* Language toggle */
const currentLang = ref("en");
const langData = {
  ka: {
    name: "ეკატო მაყაშვილი",
    profession: "ფულ სტეკ დეველოპერი",
    projectsTitle: "პროექტები",
    proj1: "პროექტი 1",
    proj2: "პროექტი 2",
    proj3: "პროექტი 3",
    more: "სრულად",
    footer: "© 2025 ეკატო მაყაშვილი. ყველა უფლება დაცულია.",
  },
  en: {
    name: "Ekato Makashvili",
    profession: "Full Stack Developer",
    projectsTitle: "Projects",
    proj1: "Project 1",
    proj2: "Project 2",
    proj3: "Project 3",
    more: "For More",
    footer: "© 2025 Ekato Makashvili. All rights reserved.",
  },
};

const skills = ref([
  "Vue.js",
  "TailwindCSS",
  "Three.js",
  "Laravel",
  "API",
  "SQL",
]);

const waveY = ref(skills.value.map(() => 0));
const wavePath = ref(null);

let t = 0;
const amplitude = 15; // ტალღის სიმაღლე
const frequency = 0.002; // ტალღის სიგრძე
const speed = 0.01; // ტალღის სიჩქარე

function animateWave() {
  const waveWidth = 1440; // SVG width
  const waveHeight = 60; // base wave amplitude reference
  const amplitude = 15; // skill box + wave amplitude
  const speed = 0.01; // controls wave speed
  t += speed;

  // Animate the wave path
  if (wavePath.value) {
    const cp1 = 360;
    const cp2 = 1080;

    const newD = `
      M0,${30 + Math.sin(t) * amplitude} 
      C${cp1},${90 + Math.sin(t + 1) * amplitude} 
       ${cp2},${-30 + Math.sin(t + 2) * amplitude} 
       1440,${30 + Math.sin(t + 3) * amplitude} 
      L1440,150 L0,150 Z
    `;
    wavePath.value.setAttribute("d", newD);
  }

  // Animate skill boxes along same sine trajectory
  skills.value.forEach((_, i) => {
    // calculate X-position fraction for each skill box
    const fraction = i / (skills.value.length - 1);
    // map fraction to wave sine for exact sync
    const phase = fraction * Math.PI * 2;
    waveY.value[i] = Math.sin(t + phase) * amplitude;
  });

  requestAnimationFrame(animateWave);
}

onMounted(() => {
  animateWave();
});
const texts = computed(() => langData[currentLang.value]);
const toggleLanguage = () =>
  (currentLang.value = currentLang.value === "ka" ? "en" : "ka");

/* Projects */
const projects = computed(() => [
  {
    img: "https://source.unsplash.com/400x250/?website",
    title: texts.value.proj1,
  },
  { img: "https://source.unsplash.com/400x250/?app", title: texts.value.proj2 },
  {
    img: "https://source.unsplash.com/400x250/?technology",
    title: texts.value.proj3,
  },
]);

/* Floating particles */
const particles = ref([]);
onMounted(() => {
  for (let i = 0; i < 50; i++) {
    const size = Math.random() * 20 + 5;
    const left = Math.random() * 100;
    const duration = Math.random() * 20 + 10;
    const delay = Math.random() * 10;
    particles.value.push({
      style: {
        width: `${size}px`,
        height: `${size}px`,
        left: `${left}%`,
        bottom: "-30px",
        animation: `rise ${duration}s linear infinite`,
        animationDelay: `${delay}s`,
      },
    });
  }
});

/* Cursor parallax */
const mouseX = ref(50),
  mouseY = ref(50);
const onMouseMove = (e) => {
  mouseX.value = (e.clientX / window.innerWidth) * 100;
  mouseY.value = (e.clientY / window.innerHeight) * 100;
};

/* Scroll animations */
onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) entry.target.classList.add("show");
      });
    },
    { threshold: 0.1 }
  );

  document.querySelectorAll(".fade-up").forEach((el) => observer.observe(el));
});
</script>

<style>
/* Gradient animation */
@keyframes gradientShift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
.animate-gradient {
  background-size: 600% 600%;
  animation: gradientShift 20s ease infinite;
}

/* Particles rising */
@keyframes rise {
  0% {
    transform: translateY(0);
    opacity: 0.2;
  }
  50% {
    opacity: 0.3;
  }
  100% {
    transform: translateY(-120vh);
    opacity: 0;
  }
}
.skills-container {
  position: relative;
  z-index: 10;
}

.skill-box {
  background: rgba(255, 255, 255, 0.15);
  color: #e2dfd2;
  font-weight: 600;
  border-radius: 50px;
  padding: 12px 24px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 6px 18px rgba(14, 165, 233, 0.1);
  transition: transform 0.1s linear;
}

.wave-separator {
  bottom: 0;
  left: 0;
  width: 100%;
  height: 80vh;
}

/* Hover ეფექტი */
.skill-box:hover {
  transform: scale(1.08);
  box-shadow: 0 12px 30px rgba(14, 165, 233, 0.22);
}

/* სინუსოიდური ტალღა */
@keyframes wave {
  0% {
    transform: translateY(0);
  }
  25% {
    transform: translateY(-6px);
  }
  50% {
    transform: translateY(0);
  }
  75% {
    transform: translateY(6px);
  }
  100% {
    transform: translateY(0);
  }
}

/* Fade-up (reuse შენი observer-ით) */
.fade-up {
  opacity: 0;
  transform: translateY(20px);
  transition: all 900ms cubic-bezier(0.16, 0.84, 0.24, 1);
}
.fade-up.show {
  opacity: 1;
  transform: translateY(0);
}
</style>
