<script lang="ts">
export default {
  inheritAttrs: false,
};
</script>

<script setup lang="ts">
interface Props {
  modelValue: string | number;
  data: {
    value: string | number;
    label: string;
  }[];
  optClass?: string;
}

const { modelValue = '', data, optClass = '' } = defineProps<Props>();
const emit = defineEmits(['update:modelValue']);
</script>

<template>
  <div class="flex flex-col w-full">
    <label class="font-semibold"> {{ $attrs.label }} </label>
    <select
      v-bind="$attrs"
      class="border border-solid rounded max-w-xs border-slate-300 p-2  dark:text-black"
      :class="{ 'bg-gray-300': $attrs.disabled }"
      :value="modelValue"
      @input="emit('update:modelValue', ($event.target as HTMLSelectElement).value)"
    >
      <option
        v-for="option in data"
        :key="option.value"
        :class="optClass"
        :value="option.value"
      >
        {{ option.label }}
      </option>
    </select>
    <div v-if="$attrs.error" class="text-red-600">
      {{ $attrs.error }}
    </div>
  </div>
</template>
