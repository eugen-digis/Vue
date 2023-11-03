<template>
  <!-- Aatlas Button -->
  <button
    class="aatlas-button"
    :class="{ 'aatlas-button--disabled': isButtonDisabled, 'aatlas-button--outline': isButtonOutline, 'aatlas-button--light': isButtonLight, 'aatlas-button--small': isButtonSmall, 'aatlas-button--extra-small': isButtonExtraSmall }"
    :disabled="isButtonDisabled"
    @click="manageClick"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: 'AatlasButton',
  props: {
    valid: {
      type: Boolean,
      required: false,
      default: true
    },
    outline: {
      type: Boolean,
      required: false
    },
    light: {
      type: Boolean,
      required: false
    },
    small: {
      type: Boolean,
      required: false
    },
    extraSmall: {
      type: Boolean,
      required: false
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  computed: {
    // Check if Button Should Be Disabled
    isButtonDisabled () {
      return !this.valid || this.disabled
    },
    // Check if Button Should be Outlined
    isButtonOutline () {
      return this.outline
    },
    isButtonLight () {
      return this.light
    },
    isButtonSmall () {
      return this.small
    },
    isButtonExtraSmall () {
      return this.extraSmall
    }
  },
  methods: {
    // Manage Click
    manageClick () {
      this.$emit('click')
    }
  }
}
</script>

<style scoped lang="sass">
.aatlas-button
  @apply outline-none text-sm text-white tracking-widest
  border-radius: 1.375rem
  min-height: 2.75rem
  min-width: 200px
  padding: 0 15px
  transition: all .2s ease-in-out
  @screen max-sm
    min-width: 178px

  @media screen and (max-width: 420px)
    @apply w-full
    min-width: auto

  &:hover
    @apply bg-orange-500
  &:active
    @apply bg-green-900 text-white

  &--outline
    @apply text-green-900 bg-transparent border border-green-900
    &:focus,
    &:hover
      @apply bg-green-900 opacity-100 text-white

  &--light
    @apply text-white bg-transparent border border-white
    &:focus,
    &:hover
      @apply bg-white opacity-100 text-green-900

  &--disabled
    @apply bg-grey-150

    &:hover
      @apply bg-grey-100
      box-shadow: none
      transform: none

    &.post-comment-btn
      @apply bg-brand-orange-light text-white #{!important}

      &:hover
        @apply bg-brand-orange-light #{!important}

  &.post-comment-btn
    &:hover
      @apply bg-brand

  &--small
    @apply px-5 py-2
    font-size: 13px
    min-height: 10px
    min-width: auto
    @screen max-sm
      @apply px-3

  &--extra-small
    @apply px-2 py-1
    font-size: 11px
    min-height: 10px
    min-width: auto

@screen max-md
  .post-comment-btn
    @apply py-1
    font-size: 12px
</style>
