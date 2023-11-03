<template>
  <!-- Aatlas Input -->
  <div :class="['aatlas-input', { 'input-icon': icon }]">
    <!-- Aatlas Input Label + Input -->
    <div class="aatlas-input--form-group">
      <!-- Aatlas Textarea -->
      <!-- :name="`${name}-${uuid}`" - unique input name to prevent browser autocomplete -->
      <template v-if="textArea">
        <textarea
          :rows="rows"
          class="aatlas-input--input textarea"
          :class="{
            'aatlas-input--input-disabled': getIsDisabled,
            'aatlas-input--input-outlined': getIsOutlined,
            'aatlas-input--input-rounded': getIsRounded,
            'aatlas-input--input-text': getTextStyle
          }"
          :name="`${name}-${uuid}`"
          :placeholder="getLabel"
          :value="getValidValue"
          :type="getInputType"
          :autocomplete="autocomplete"
          :disabled="getIsDisabled"
          @input="updateInput"
        />
        <label
          v-if="icon"
          class="aatlas-input--label ml-8 mt-1"
          :for="name"
        >
          <i :class="['textarea-icon icon', icon]" />
        </label>
      </template>
      <div
        v-else
        class="flex flex-row-reverse"
      >
        <input
          class="aatlas-input--input"
          :class="{
            'aatlas-input--input-disabled': getIsDisabled,
            'aatlas-input--input-outlined': getIsOutlined,
            'aatlas-input--input-rounded': getIsRounded,
            'aatlas-input--input-text': getTextStyle
          }"
          :name="`${name}-${uuid}`"
          :placeholder="getPlaceholder"
          :value="getValidValue"
          :type="getInputType"
          :disabled="getIsDisabled"
          :readonly="readonly"
          :maxlength="getMaxlength"
          :autocomplete="autocomplete"
          @input="updateInput"
          @focus="onInputFocus"
        >
        <template v-if="icon">
          <span
            v-if="name === 'memoryTitle'"
            class="text-icon icon"
          >{{ icon }}</span>
          <i
            v-else
            :class="['mt-3 icon', icon]"
          />
        </template>
        <!-- Aatlas Input Label -->
        <label
          class="aatlas-input--label"
          :for="name"
        >
          <template v-if="!textArea">
            {{ getLabel }}
          </template>
          <!-- Aatlas Input Tooltip -->
          <i
            v-if="getTooltip"
            v-tooltip="getTooltip"
            class="fas fa-question-circle icon aatlas-input--tooltip"
          />
        </label>
      </div>
      <div class="aatlas-input--footnote">
        {{ getFootnote }}
      </div>
    </div>
    <!-- Aatlas Input Valid Check -->
    <p
      v-if="getErrorMessage"
      class="aatlas-input--error"
      :class="{
        'aatlas-input--error-text': getTextStyle
      }"
    >
      {{ getErrorMessage }}
    </p>
  </div>
</template>

<script>

export default {
  name: 'AatlasInput',
  model: {
    event: 'updateInput'
  },
  props: {
    label: {
      type: String,
      required: false,
      default: ''
    },
    placeholder: {
      type: String,
      required: false,
      default: ' '
    },
    name: {
      type: String,
      required: false,
      default: ''
    },
    tooltip: {
      type: String,
      required: false,
      default: ''
    },
    inputType: {
      type: String,
      required: false,
      default: 'text'
    },
    valid: {
      type: [Boolean, Object],
      required: false,
      default: true
    },
    errorMessage: {
      type: String,
      required: false,
      default: ''
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false
    },
    outlined: {
      type: Boolean,
      required: false,
      default: false
    },
    textStyle: {
      type: Boolean,
      required: false,
      default: false
    },
    rounded: {
      type: Boolean,
      required: false,
      default: false
    },
    value: {
      type: String,
      required: false,
      default: ''
    },
    textArea: {
      type: Boolean,
      required: false,
      default: false
    },
    footnote: {
      type: String,
      required: false,
      default: ''
    },
    readonly: {
      type: Boolean,
      required: false,
      default: false
    },
    maxlength: {
      type: Number,
      required: false,
      default: 200
    },
    rows: {
      type: Number,
      required: false,
      default: 3
    },
    required: {
      type: Boolean,
      required: false,
      default: false
    },
    icon: {
      type: String,
      required: false,
      default: ''
    },
    autocomplete: {
      type: String,
      required: false,
      default: 'non' // random string for browsers to ignore autocomplete
    }
  },
  computed: {
    // Get Input Label
    getLabel () {
      return this.label
    },
    // Get Input Placeholder
    getPlaceholder () {
      return this.placeholder
    },
    // Get Input Type
    getInputType () {
      return this.inputType
    },
    // Get if Input is Valid
    getIsValid () {
      return this.valid
    },
    // Get if Input is Disabled
    getIsDisabled () {
      return this.disabled
    },
    getIsOutlined () {
      return this.outlined
    },
    getTextStyle () {
      return this.textStyle
    },
    getIsRounded () {
      return this.rounded
    },
    getTooltip () {
      return this.tooltip
    },
    getValidValue () {
      if (this.getMaxlength && this.value) {
        return this.value.substring(0, this.getMaxlength)
      }
      return this.value
    },
    getFootnote () {
      return this.footnote
    },
    getMaxlength () {
      return this.maxlength
    },
    getErrorMessage () {
      if (this.value === '' && this.required) {
        return this.$contentful.uiStrings.requiredFieldErrorMessage
      }
      if (!this.getIsValid && this.value !== null && this.errorMessage) {
        return this.errorMessage
      }
      if (this.getMaxlength && this.value && this.value.length >= this.getMaxlength) {
        return this.$contentful.uiStrings.errorMessageCharacterLimit
      }
      return ''
    }
  },
  methods: {
    // Update Input
    updateInput (e) {
      this.$emit('updateInput', e.target.value.replace(/\s*$/, ''))
    },
    onInputFocus (e) {
      if (this.value === null) {
        this.$emit('updateInput', '')
      } else if (this.value) {
        e.target.select()
      }
    }
  }
}
</script>

<style scoped lang="sass">
.aatlas-input
  @apply mx-auto mb-3 mt-5 relative text-left text-lg

  &--label
    @apply absolute font-body left-0 pointer-events-none text-base text-grey-500 z-10
    top: 6px
    transition: .3s

  &--error
    @apply text-brand text-xs font-body pt-3

  &--error-text
    @apply text-center pt-0

  &--form-group
    @apply relative

  &--input
    @apply bg-transparent border-grey-500 border-solid font-body outline-none pr-6 py-1 relative text-black text-lg w-full
    border-bottom-width: 1px
    transition: .3s

    &::placeholder
      @apply font-normal text-base text-grey-500

    &:focus
      &::placeholder
        @apply text-grey-200

    &:not(textarea):focus ~ .aatlas-input--label,
    &:not(textarea):not(:placeholder-shown) ~ .aatlas-input--label
      @apply text-xs
      top: -10px

    & ~ .icon
      @apply text-grey-500
      transition: .3s

    &:focus ~ .icon,
    &:not(:placeholder-shown) ~ .icon
      @apply text-green-900

    &-disabled
      @apply text-grey-500

    &-outlined
      @apply bg-white rounded-lg pl-4
      border: .8px solid gray

      & ~ .aatlas-input--label
        @apply bg-white px-1
        left: 17px
        top: 7px

    &-text
      @apply bg-transparent border-2 border-transparent rounded-lg text-center

      &:focus
        @apply border-brand-black
    &-rounded
      @apply pl-5 rounded-full

  & .text-icon
    @apply font-black text-xl
    margin-top: 3px

  & .textarea-icon
    margin-left: -20px

  & .textarea
    @apply bg-white mt-0 pt-2 pb-4 pl-4 rounded-t-lg

    &:focus ~ .aatlas-input--label,
    &:not(:placeholder-shown) ~ .aatlas-input--label
      @apply text-green-900

  @apply text-xs
    top: -10px

  &--tooltip
    @apply ml-1 pointer-events-auto

  &--footnote
    @apply w-full text-sm text-green-900

  &--valid
    @apply absolute mt-3 right-0 text-lg
    top: 0

  &.input-icon
    .aatlas-input--input:not(textarea)
      margin-left: 18px

    .aatlas-input--label
      margin-left: 28px

    .textarea
      box-sizing: border-box
      padding-left: 37px

      & ~ .aatlas-input--label
        @apply ml-8
</style>
