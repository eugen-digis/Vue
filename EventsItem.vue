<template>
  <!-- Events Item -->
  <div class="events-item">
    <!-- Events Item Information -->
    <div class="events-item--information">
      <AatlasEntityItemHeader
        :title="getEventTitle"
        :start-date="getEventDate(event).startDate"
        :end-date="getEventDate(event).endDate"
        :location="getEntityLocation(event)"
        :is-group-event="isGroupEventMode"
        :contributor-name="getEventContributorName"
        :contributor-avatar="getEventContributorAvatar"
        :contributed-at="getEventContributionDate"
        :text="getEventMemoryText"
        :event="event"
        :is-event="true"
      />

      <!-- Events Item Media -->
      <GalleryThumbnails
        v-if="getEventMedia"
        :media="getEventMedia.media"
        :media-preview="getEventMedia.contentsPreviews"
        is-use-gallery
        :is-group-event-mode="isGroupEventGallery"
        :event-id="event.id"
      />

      <!-- Events Item Comments -->
      <div class="events-item--information--comments">
        <AatlasComments
          v-if="!isReadOnlyMode && getEventCommentsEntityId"
          :entity-id="getEventCommentsEntityId"
          :entity-type="getCommentsEntityType"
          :entity-content-author-id="event.author.id"
        />
      </div>
      <AatlasButton
        v-if="!isReadOnlyMode && event.isGroupEvent"
        class="events-item--information--add-to-event-btn"
        label="Respond"
        outline
        @click="handlerClickAddToEvent"
      >
        {{ $contentful.uiStrings.addToEventButtonCTA }}
      </AatlasButton>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import UIStrings from '../../config/UIStrings'
import getLocationMixin from '@/mixins/getLocation'
import groupMemoriesMixin from '@/mixins/groupMemories'
import memoriesMixin from '@/mixins/memories'
import eventItemHelperMixin from '@/mixins/eventItemHelper'
import aatlasRolesMixin from '@/mixins/aatlasRoles'

export default {
  name: 'EventsItem',
  components: {
    AatlasButton: () => import('@/components/globals/AatlasButton'),
    AatlasComments: () => import('@/components/globals/AatlasComments'),
    AatlasEntityItemHeader: () => import('@/components/globals/AatlasEntityItemHeader'),
    GalleryThumbnails: () => import('@/components/globals/GalleryThumbnails.vue')
  },
  mixins: [
    getLocationMixin,
    groupMemoriesMixin,
    memoriesMixin,
    eventItemHelperMixin,
    aatlasRolesMixin
  ],
  props: {
    event: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      eventsUI: UIStrings.eventsUI
    }
  },
  computed: {
    ...mapGetters('AatlasRepositoryModule', ['getSelectedAatlas']),
    ...mapGetters('AuthModule', ['getUser'])
  },
  methods: {
    ...mapActions('AatlasModalsModule', ['setAddEventModalOpen', 'setAddContentModalType']),
    ...mapActions('AatlasRepositoryModule', ['setCurrentContent']),
    async handlerClickAddToEvent () {
      await this.setCurrentContent(this.event)
      await this.setAddContentModalType('event')
      this.setAddEventModalOpen(true)
    },
    getEventDate (date) {
      if (this.event.isGroupEvent) {
        return this.event
      } else if (!this.event.isGroupEvent) {
        return this.getMemoryDates(this.event.memories[0])
      }
    }
  }
}
</script>

<style scoped lang="sass">
.events-item
  @apply flex flex-wrap my-5

  &--information
    @apply mt-2 w-full
    margin-left: 55px

    &--title
      @apply mb-3 typeface-h3

    &--years
      @apply typeface-event-caption

    &--location
      @apply font-normal mb-5 mt-1 text-brand-light-gray typeface-event-caption
      letter-spacing: 0.64px

      &--icon
        @apply inline mr-1
        height: 18px
        width: 14px

    &--description
      @apply leading-loose mt-5 typeface-body w-11/12
      display: -webkit-box
      overflow: hidden
      text-overflow: ellipsis
      -webkit-box-orient: vertical
      -webkit-line-clamp: 3

    &--photos
      @apply my-5 flex
      min-height: 160px

      img
        @apply mr-2 rounded

    &--comments
      @apply my-4 typeface-comments

      &--amount
        @apply typeface-comments

      &--avatars
        @apply flex float-right

        img
          @apply rounded-full
          height: 24px
          width: 24px

        &--placeholder
          @apply bg-grey-500 flex flex-wrap items-center justify-center ml-1 rounded-full text-white text-xs
          height: 24px
          width: 24px

    &--add-to-event-btn
      @apply px-2 py-1 #{!important}
      font-size: 10px !important
      height: 25px
      letter-spacing: 0.86px !important
      min-height: initial !important
</style>
