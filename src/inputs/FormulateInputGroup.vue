<template>
  <div class="formulate-input-group">
    <template
      v-if="subType !== 'grouping'"
    >
      <FormulateInput
        v-for="optionContext in optionsWithContext"
        :key="optionContext.id"
        v-model="context.model"
        v-bind="optionContext"
        :disable-errors="true"
        class="formulate-input-group-item"
        @blur="context.blurHandler"
      />
    </template>
    <template
      v-else
    >
      <FormulateGrouping
        :context="context"
      >
        <slot />
      </FormulateGrouping>
    </template>
  </div>
</template>

<script>
export default {
  name: 'FormulateInputGroup',
  props: {
    context: {
      type: Object,
      required: true
    }
  },
  computed: {
    options () {
      return this.context.options || []
    },
    subType () {
      return (this.context.type === 'group') ? 'grouping' : 'inputs'
    },
    optionsWithContext () {
      const {
        // The following are a list of items to pull out of the context object
        options,
        labelPosition,
        attributes: { id, ...groupApplicableAttributes },
        classification,
        blurHandler,
        performValidation,
        hasValidationErrors,
        getValidationErrors,
        validationErrors,
        setErrors,
        visibleValidationErrors,
        component,
        hasLabel,
        slotComponents,
        isSubField,
        ...context
      } = this.context
      return this.options.map(option => this.groupItemContext(
        context,
        option,
        groupApplicableAttributes
      ))
    }
  },
  methods: {
    groupItemContext (context, option, groupAttributes) {
      const optionAttributes = {}
      const ctx = Object.assign({}, context, option, groupAttributes, optionAttributes)
      return ctx
    }
  }
}
</script>