<template>
  <div class="ty-select">
    <label :for="id" v-if="formSchema.theme.inputs.labelStyle==='top'" class="ty-label ty-label-top">{{label}}</label>
    <q-select
      :label = "formSchema.theme.inputs.labelStyle !=='top' ? label : undefined"
      :outlined = "formSchema.theme.inputs.style==='full'"
      :borderless = "formSchema.theme.inputs.borderless"
      :stack-label = "formSchema.theme.inputs.labelStyle==='stacked'"
      :input-style = "inputStyle"
      :dense = "formSchema.theme.inputs.dense"
      :dark = "formSchema.theme.inputs.darkMode"
      :clearable = "behavior.clearable"
      :counter = "behavior.counter"
      :placeholder = "placeholder"
      :for = "id"
      :readonly = "behavior.readOnly"
      :disable = "behavior.disabled"
      v-model = "modelValueRef"
      @update:model-value = "onUpdate"
      :options = "options"
      :multiple = "behavior.multiple"
      :use-chips = "behavior.multiple"
      :name = "name"
      class="ty-select"
      bottom-slots
    >
      <template v-slot:hint>
        <div class="ty-hint" :class="formSchema.theme.card.dark ?'text-white':'text-black'">{{hint}}</div>
      </template>
    </q-select>
  </div>

</template>

<script>
import { inject, computed, ref } from 'vue';

export default {
  name: 'TySelect',
  components: {
  },
  props: {
    type: {
      type: String /* select */
    },
    name: {
      type: String,
      required: true
    },
    label: String,
    hint: String,
    placeholder: String,
    behavior: {
      type: Object, /* readOnly, clearable, disabled, displayed, counter */
      default: () => ({})
    },
    options: {
      type: Array,
      default: () => []
    },
    modelValue: [Array, Object]
  },
  emits: ['update:modelValue'],
  setup (props, {emit}) {
    const formSchema = inject('formSchema');
    const modelValueRef = ref(props.modelValue)
    const id = computed(() => {
      return props.type + '_' + props.name
    });
    const inputStyle = computed(() => {
      return {color: formSchema.value.theme.inputs.textColor ? formSchema.value.theme.inputs.textColor : undefined}
    })
    const onUpdate = (evt) => {
      emit('update:modelValue', evt)
    }
    return {
      formSchema,
      id,
      inputStyle,
      modelValueRef,
      onUpdate
    }
  }
}
</script>
