<script setup lang="ts">
import { useStorage } from '@vueuse/core'


const sliderValue = useStorage('aare-min-max', [17,22])

type AareGuruData = {
    aare: number
}

const {data, error} = await useFetch<AareGuruData>("https://aareguru.existenz.ch/v2018/today?city=bern&app=geit.jan.go.bade&version=1.0.0")

const aareTemp = computed(() => data?.value?.aare);
const max = computed(() => sliderValue.value[1]);
const min = computed(() => sliderValue.value[0]);

const tooHot = computed(() => {
    if (aareTemp.value && min.value && max.value) {
        if (aareTemp.value > max.value)  {
            {
                return true
            }
        }}
        return false
})

const tooCold = computed(() => {
    if (aareTemp.value && min.value && max.value) {
        if (aareTemp.value < min.value)  {
            {
                return true
            }
        }}
        return false
})


</script>

<template>
  <div class="min-h-screen flex flex-col relative overflow-hidden bg-gradient-to-br from-cyan-50 via-sky-100 to-blue-200 dark:from-slate-950 dark:via-cyan-950 dark:to-blue-950 transition-colors duration-500">
    <!-- Background decorative floating shapes -->
    <div class="absolute -top-20 -left-20 w-96 h-96 bg-cyan-300/40 dark:bg-cyan-600/20 rounded-full blur-3xl pointer-events-none animate-pulse"></div>
    <div class="absolute top-1/3 -right-20 w-[30rem] h-[30rem] bg-blue-400/30 dark:bg-blue-600/20 rounded-full blur-3xl pointer-events-none"></div>
    <div class="absolute -bottom-32 left-1/4 w-[28rem] h-[28rem] bg-teal-300/30 dark:bg-teal-600/20 rounded-full blur-3xl pointer-events-none"></div>

    

    <main class="flex-1 flex flex-col items-center justify-center pt-10 pb-16 px-6 relative z-10">
      <!-- Header Section with Icons -->
      <div class="flex flex-col items-center gap-6 text-center max-w-4xl mx-auto">
        <div class="flex items-center gap-4">
          <div class="p-3 bg-cyan-500 text-white rounded-2xl shadow-lg shadow-cyan-500/40 rotate-[-6deg] hover:rotate-0 transition-transform duration-300">
            <UIcon name="i-heroicons-sun" class="w-10 h-10" />
          </div>
          <h1 class="text-7xl md:text-8xl font-black text-slate-900 dark:text-white tracking-tight drop-shadow-sm">
            Geht Jan Baden?
          </h1>
         
          <div class="p-3 bg-blue-500 text-white rounded-2xl shadow-lg shadow-blue-500/40 rotate-[6deg] hover:rotate-0 transition-transform duration-300">
            <UIcon name="i-heroicons-face-smile" class="w-10 h-10" />
          </div>
        </div>

        <p class="text-2xl font-bold text-slate-700 dark:text-cyan-200 flex items-center gap-2">
          <UIcon name="i-heroicons-question-mark-circle" class="w-7 h-7 text-cyan-600 dark:text-cyan-400" />
          Bei welchen Temperaturen gehst du baden?
        </p>
         <UIcon v-if="tooHot" name="i-ph:thermometer-hot-fill" class="size-40" />
         <UIcon v-if="tooCold" name="i-ph:thermometer-cold-fill" class="size-40" />
         <UIcon v-if="!tooCold && !tooHot" name="i-hugeicons:swimming" class="size-40" />
      </div>

      <!-- Main Interactive Box -->
      <div class="flex flex-col items-center gap-8 mt-16 w-full max-w-7xl bg-white/70 dark:bg-slate-900/70 backdrop-blur-xl p-10 rounded-3xl shadow-2xl border border-white/60 dark:border-slate-800/80">
        <!-- Range indicators with icons -->
        <div class="flex justify-between w-[950px] text-2xl font-black text-slate-900 dark:text-white px-2">
          <div>
            <span class="flex items-center gap-2">
            min. Wassertemp.
          </span>
          <p>{{ min }}°</p>
          </div>

          <div>
          <span class="flex items-center gap-2">
            max. Wassertemp.
          </span>
          <p>{{ max }}°</p>
          </div>
        </div>

        <!-- Slider and Scale Container -->
        <div class="w-[950px] relative pt-6 pb-8">
          <USlider v-model="sliderValue" :min="1" :max="25" :step="1" size="xl" class="w-full" tooltip />

        </div>
      </div>
    </main>
  </div>
</template>