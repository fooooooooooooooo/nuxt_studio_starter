<script setup lang="ts">
const route = useRoute()

const { data: page } = await useAsyncData('page-' + route.path, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

function logEvents(e: { data: unknown }) {
  console.info('message:', e.data);
}

onMounted(() => {
  window.addEventListener('message', logEvents);
});

onUnmounted(() => {
  window.removeEventListener('message', logEvents);
});
</script>

<template>
  <ContentRenderer
    v-if="page"
    :value="page"
  />
</template>
