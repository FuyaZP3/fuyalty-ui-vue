<script setup lang="ts">
import { computed } from 'vue'

interface Props {
  label?: string
  variant?: 'filled' | 'outlined'
  color?: 'success' | 'warning' | 'danger'
  disabled?: boolean
  type?: 'button' | 'submit' | 'reset'
}

const props = defineProps<Props>()

const classes = computed(() => {
  const base = 'inline-flex justify-center align-middle items-center px-4 py-2'

  const colors: Record<NonNullable<Props['color']>, string> = {
    success: 'bg-green-500 hover:bg-green-600 active:bg-green-700 text-white disabled:bg-gray-500',
    warning: 'bg-yellow-500 hover:bg-yellow-600 active:bg-yellow-700 text-black',
    danger: 'bg-red-500 hover:bg-red-600 active:bg-red-700 text-white'
  }
  
  const disabled = props.disabled ? 'opacity-50 cursor-not-allowed' : ''

  return [base, colors[props.color ?? 'primary'], disabled]
})
</script>

<template>
  <button 
    :class="classes"
    :disabled="disabled"
  >
    <span>
      <slot>{{ label }}</slot>
    </span>
  </button>
</template>