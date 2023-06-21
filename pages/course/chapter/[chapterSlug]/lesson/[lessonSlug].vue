<script setup lang="ts">
import { useRoute } from "vue-router";
import { useCourse } from "../../../../../composables/useCourse";
import { computed } from "vue";

const route = useRoute();
const course = useCourse();

const chapter = computed(() =>
  course.chapters.find((chapter) => chapter.slug === route.params.chapterSlug)
);

const lesson = computed(() =>
  chapter?.value?.lessons.find(
    (lesson) => lesson.slug === route.params.lessonSlug
  )
);
</script>

<template>
  <div>
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter?.number }} - {{ lesson?.number }}
    </p>
    <h2 class="my-0">{{ lesson?.title }}</h2>
    <div class="flex space-x-4 mt-2 mb-8">
      <a
        v-if="lesson?.sourceUrl"
        target="_blank"
        class="font-normal text-md text-gray-500"
        :href="lesson?.sourceUrl"
        >Download Source Code</a
      >
      <a
        v-if="lesson?.downloadUrl"
        target="_blank"
        class="font-normal text-md text-gray-500"
        :href="lesson?.downloadUrl"
        >Download video</a
      >
    </div>
    <VideoPlayer v-if="lesson?.videoId" :videoId="lesson.videoId" />
    <p>{{ lesson?.text }}</p>
  </div>
</template>