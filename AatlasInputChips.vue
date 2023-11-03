<template>
  <!-- Aatlas Input -->
  <div class="aatlas-input-chip">
    <div class="aatlas-input-chip--wrap">
      <!-- Aatlas Input Label + Input -->
      <div
        v-for="(chip, i) of emails"
        :key="chip.label"
        class="aatlas-input-chip--chip"
      >
        {{ chip }}
        <i
          class="fas fa-times-circle"
          @click="deleteChip(i)"
        />
      </div>
      <!-- Aatlas Input Input -->
      <!-- :name="`${name}-${uuid}`" - unique input name to prevent browser autocomplete -->
      <input
        :id="name"
        class="aatlas-input-chip--input"
        :class="{
          'aatlas-input-chip--input-disabled': getIsDisabled,
          'aatlas-input-chip--input-outlined': getIsOutlined,
          'aatlas-input-chip--input-rounded': getIsRounded
        }"
        :name="`${name}-${uuid}`"
        :placeholder="getPlaceholder"
        :value="getValue"
        :type="getInputType"
        :disabled="getIsDisabled"
        :autocomplete="autocomplete"
        @input="updateInput"
        @keydown="addChip"
        @keydown.delete="backspaceDelete"
      >
      <!-- Aatlas Input Label -->
      <label
        class="aatlas-input-chip--label"
        :for="name"
      >
        {{ getLabel }}
        <!-- Aatlas Input Tooltip -->
        <i
          v-if="getTooltip"
          v-tooltip="getTooltip"
          class="fas fa-question-circle aatlas-input-chip--tooltip"
        />
      </label>
    </div>
    <div class="aatlas-input-chip--footnote">
      {{ getFootnote }}
    </div>
    <!-- Aatlas Input Valid Check -->
    <i
      v-if="getIsValid"
      class="fas fa-check aatlas-input-chip--valid"
    />
  </div>
</template>

<script>

export default {
  name: 'AatlasInputChips',
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
      type: Boolean,
      required: false,
      default: false
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
    emails: {
      type: Array,
      required: true,
      default: () => {}
    },
    isEmailEntered: {
      type: Boolean,
      required: true,
      default: false
    },
    footnote: {
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
  data () {
    return {
      emailRegex: /^\s?\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,10})+$/
    }
  },
  computed: {
    // Get Input Label
    getLabel () {
      return this.label
    },
    getFootnote () {
      return this.footnote
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
    getIsRounded () {
      return this.rounded
    },
    getTooltip () {
      return this.tooltip
    },
    getValue () {
      return this.value.replace(',', ' ')
    }
  },
  methods: {
    // Update Input
    updateInput (e) {
      this.$emit('updateInput', e.target.value)

      if (this.emailRegex.test(e.target.value)) {
        this.$emit('update:isEmailEntered', true)
      } else {
        this.$emit('update:isEmailEntered', false)
      }
    },
    addChip (event) {
      const email = this.getValue.replace(' ', '')

      if ((event.key === ' ' || event.key === ',' || event.key === 'Enter') && this.emailRegex.test(email)) {
        if (this.emails.indexOf(email) !== -1) {
          this.$emit('update:value', '')
          return
        }
        this.emails.push(email)
        this.$emit('update:value', '')
        this.$emit('update:emails', this.emails)
      }
    },
    deleteChip (index) {
      this.emails.splice(index, 1)
    },
    backspaceDelete () {
      this.getValue === '' && this.emails.splice(this.emails.length - 1)
    }
  }
}
</script>

<style scoped lang="sass">
.aatlas-input-chip
  &--wrap
    @apply flex flex-wrap mx-auto my-1 relative text-left text-lg
    border-bottom-width: .2px
    padding-bottom: 7px

  &--chip
    @apply bg-grey-75 flex px-3 py-1 rounded-full text-sm z-10
    margin: 7px 7px 0 0
    i
      @apply cursor-pointer ml-2
      margin-top: 5px

  &--label
    @apply absolute font-body left-0 pointer-events-none text-base text-grey-400
    top: 10px
    transition: .3s

  &--form-group
    @apply relative

  &--input
    @apply bg-transparent font-body outline-none pt-2 relative text-black text-lg w-full
    min-width: 100px
    flex: 1
    transition: .3s

    &:focus ~ .aatlas-input-chip--label,
    &:not(:placeholder-shown) ~ .aatlas-input-chip--label
      @apply text-xs
      top: -10px

    &-disabled
      @apply text-grey-500

    &-outlined
      @apply bg-white rounded-lg pl-4
      border: .8px solid gray

      &::placeholder
        @apply font-normal text-brand-lighter-gray
        font-size: 15px
        letter-spacing: 0.54px

      & ~ .aatlas-input-chip--label
        @apply bg-white px-1
        left: 17px
        top: 7px

    &-rounded
      @apply pl-5 rounded-full

  &--tooltip
    @apply ml-1 pointer-events-auto

  &--footnote
    @apply text-sm w-full text-green-900

  &--valid
    @apply absolute mt-3 right-0 text-lg
    top: 15px
</style>
