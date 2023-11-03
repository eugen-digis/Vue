<template>
  <!-- Chapter Item -->
  <div
    class="chapters-item"
    @click="goToFirstChapterEvent"
  >
    <!-- Chapter Left Pane -->
    <div class="chapters-item--left-pane">
      <!-- Chapter Chapter Number -->
      <div class="chapters-item--number">
        CHAPTER {{ getChapterNumber }}
      </div>

      <!-- Chapter Title -->
      <div class="chapters-item--title">
        {{ getChapterTitle }}
      </div>

      <!-- Chapter Events Counter -->
      <div class="chapters-item--events-counter">
        {{ getEventsCounter }} EVENTS
      </div>
    </div>

    <!-- Chapter Years -->
    <div class="chapters-item--years">
      {{ getEventYearsRangeString(chapter) }}
      <i
        v-if="hasEvents"
        class="fas fa-chevron-right ml-2"
      />
      <i
        v-if="!hasEvents"
        class="chapters-item--years--empty-icon"
      />
    </div>
  </div>
</template>

<script>
import UIStrings from '../../config/UIStrings'
import entityYearsMixin from '@/mixins/entityYears'

export default {
  name: 'ChaptersItem',
  mixins: [
    entityYearsMixin
  ],
  props: {
    chapter: {
      type: Object,
      required: true,
      default: () => {}
    },
    deathDate: {
      validator: (val) => {
        return val
      },
      default: null
    }
  },
  data () {
    return {
      generalUI: UIStrings.generalUI
    }
  },
  computed: {
    // Get Chapter Number
    getChapterNumber () {
      return this.chapter.chapterNumber
    },
    // Get Events Counter
    getEventsCounter () {
      return this.chapter.events.length
    },
    // Get Chapter Title
    getChapterTitle () {
      return this.chapter.name
    },
    hasEvents () {
      return !!(this.chapter.events && this.chapter.events.length > 0)
    }
  },
  methods: {
    goToFirstChapterEvent () {
      if (this.hasEvents) this.$emit('chapterIdToScroll', this.chapter.chapterNumber)
    }
  }
}
</script>

<style scoped lang="sass">
.chapters-item
  @apply bg-pink-200 border-b border-grey-300 cursor-pointer flex relative text-green-900
  padding-right: 130px

  &--left-pane
    @apply flex flex-col mb-4 mt-6 mx-4

  &--number
    @apply typeface-chapter-subtitle

  &--events-counter
    @apply font-bold typeface-chapter-subtitle

  &--title
    @apply my-1 typeface-chapter-title

  &--years
    @apply absolute mr-3 right-0 typeface-chapter-date
    top: 50%
    transform: translateY(-50%)

    &--empty-icon
      margin-left: 1.1rem
</style>
