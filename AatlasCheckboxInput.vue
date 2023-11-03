<template>
  <!-- Aatlas Checkbox Input -->
  <div class="aatlas-checkbox-input">
    <!-- Aatlas Checkbox Input Label + Input -->
    <label class="aatlas-checkbox-input--label">
      <!-- Aatlas Checkbox Input Input -->
      <input
        type="checkbox"
        class="aatlas-checkbox-input--input"
        :name="name"
        :value="getValue"
        :checked="getChecked"
        @change="updateInput"
      >
      <span
        class="checkmark"
        :class="{
          'checkmark--rounded' : getRounded,
          'checkmark--rounded-corners' : getRoundedCorners,
          'checkmark--small' : getSmall,
        }"
      />
      <!-- eslint-disable-next-line -->
      <span :class="{ 'text-black': getChecked }" v-html="getLabel" />
      <i
        v-if="tooltip"
        v-tooltip="{ content: tooltip, trigger: 'hover click'}"
        class="fas fa-question-circle aatlas-checkbox-input--tooltip"
      />
    </label>
  </div>
</template>

<script>
export default {
  name: 'AatlasCheckboxInput',
  model: {
    prop: 'checked',
    event: 'updateInput'
  },
  props: {
    label: {
      type: String,
      required: false,
      default: ''
    },
    name: {
      type: String,
      required: false
    },
    tooltip: {
      type: String,
      required: false
    },
    value: {
      type: [String, Number, Boolean],
      required: false,
      default: ''
    },
    rounded: {
      type: Boolean,
      required: false,
      default: false
    },
    roundedCorners: {
      type: Boolean,
      required: false,
      default: false
    },
    small: {
      type: Boolean,
      required: false,
      default: false
    },
    checked: {
      type: Boolean,
      required: false
    }
  },
  computed: {
    // Get Checkbox Label
    getLabel () {
      return this.label
    },
    // Get Checkbox Value,
    getValue () {
      return this.value
    },
    // Get if Checkbox is Checked
    getChecked () {
      return this.checked
    },
    getRounded () {
      return this.rounded
    },
    getRoundedCorners () {
      return this.roundedCorners
    },
    getSmall () {
      return this.small
    }
  },
  methods: {
    // Update Input
    updateInput (e) {
      this.$emit('updateInput', e.target.checked)
    }
  }
}
</script>

<style scoped lang="sass">
.aatlas-checkbox-input
  @apply inline-block mb-0 mt-4 text-grey-400 text-left tracking-wider typeface-input-radio

  &--label
    @apply block cursor-pointer relative
    padding-left: 24px
    padding-top: 5px
    user-select: none
    -moz-user-select: none
    -ms-user-select: none
    -webkit-user-select: none

    &:hover input ~ .checkmark
      @apply bg-grey-500

    input
      @apply absolute cursor-pointer h-0 mr-3 opacity-0 w-0

      &:focus ~ .checkmark
        @apply bg-grey-500

    input:checked ~ .checkmark:after
      @apply block

    input:checked ~ .checkmark
      @apply bg-black

    .checkmark
      @apply absolute bg-pink-200 border left-0 rounded
      border-width: 1.5px
      height: 20px
      top: 4px
      width: 20px

      &--rounded
        @apply rounded-full

      &--rounded-corners
        border-radius: 3px

      &--small
        height: 16px
        width: 16px
        &.checkmark:after
          height: 8px
          left: 4px
          top: 1px
          width: 5px

    .checkmark:after
      @apply absolute hidden
      content: ""

    .checkmark:after
      @apply border-solid border-white
      border-width: 0 2px 2px 0
      height: 12px
      left: 7px
      top: 2px
      transform: rotate(45deg)
      width: 5px
      -ms-transform: rotate(45deg)
      -webkit-transform: rotate(45deg)

  &--tooltip
    @apply inline ml-1
</style>
