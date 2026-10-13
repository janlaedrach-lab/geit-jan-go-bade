<script setup lang="ts">
import { useStorage } from "@vueuse/core";
import dayjs from "dayjs";

const sliderValue = useStorage("aare-min-max", [17, 22]);

type AareGuruData = {
  aare: number;
};

const { data, error } = await useFetch<AareGuruData>(
  "https://aareguru.existenz.ch/v2018/today?city=bern&app=geit.jan.go.bade&version=1.0.0",
);

const aareTemp = computed(() => data?.value?.aare);
const max = computed(() => sliderValue.value[1]);
const min = computed(() => sliderValue.value[0]);

const tooHot = computed(() => {
  if (aareTemp.value && min.value && max.value) {
    if (aareTemp.value > max.value) {
      {
        return true;
      }
    }
  }
  return false;
});

const tooCold = computed(() => {
  if (aareTemp.value && min.value && max.value) {
    if (aareTemp.value < min.value) {
      {
        return true;
      }
    }
  }
  return false;
});
</script>

<template>
  <div class="flex flex-col items-center p-12">
    <h1 class="text-3xl sm:text-6xl font-black pb-8">Geit Jan go Bade?</h1>
    <div>
      <UIcon v-if="tooHot" name="i-ph:thermometer-hot-fill" class="size-40" />
      <UIcon v-if="tooCold" name="i-ph:thermometer-cold-fill" class="size-40" />
      <UIcon v-if="!tooCold && !tooHot" name="i-hugeicons:swimming" class="size-40" />
    </div>

    <UCard class="w-full">
      <template #header>
        <div class="flex justify-between">
          <div>
            <p class="font-bold">min</p>
            <p>{{ min }}°</p>
          </div>
          <div>
            <p class="font-bold">max</p>
            <p>{{ max }}°</p>
          </div>
        </div>
      </template>
      <USlider
        v-model="sliderValue"
        :min="1"
        :max="25"
        :step="1"
        size="xl"
        class="w-full"
      ></USlider>

      <template #footer>
        <p>Stelle deinen Konfortbereich ein.</p>
      </template>
    </UCard>
  </div>

  <UFooter>
    <template #left>
      <p class="text-muted text-sm">Copyright © {{ new Date().getFullYear() }} Jan Lädrach</p>
    </template>
  </UFooter>
</template>
