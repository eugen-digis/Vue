<template>
  <!-- Memories Card -->
  <div class="contributor flex md:inline-flex md:w-1/3">
    <div
      class="contributor__avatar"
      :style="`background-image: url(${contributor.avatar})`"
    />
    <div class="contributor__wrap">
      <p class="contributor__name">
        {{ contributor.name }}
      </p>
      <p class="contributor__label">
        {{ contributor.label }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContributorCard',
  props: {
    contributor: {
      type: Object,
      required: true
    }
  }
}
</script>
<style scoped lang="sass">
.contributor
  @apply justify-start items-center my-3
  &__avatar
    @apply bg-cover bg-center rounded-full w-16 h-16
  &__wrap
    @apply ml-3
  &__name
    @apply typeface-contributor-title mb-1
  &__label
    @apply typeface-event-caption
</style>
