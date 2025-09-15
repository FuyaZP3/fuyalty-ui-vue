<script setup lang="ts">
import { computed } from 'vue'

type Colors = 'success' | 'warning' | 'danger'

interface Props {
  label?: string
  variant?: 'filled' | 'outlined'
  color?: Colors
  disabled?: boolean
  type?: 'button' | 'submit' | 'reset'
}

const props = defineProps<Props>()

const classes = computed(() => {
  const base = 'inline-flex justify-center align-middle items-center px-4 py-2'

  const colors: Record<NonNullable<Props['color']>, string> = {
    success: `
      bg-green-500 text-white
      hover:bg-green-600 active:bg-green-700
      disabled:opacity-50 disabled:hover:bg-green-500 disabled:active:bg-green-500
    `,
    warning: `
      bg-yellow-500 text-black
      hover:bg-yellow-600 active:bg-yellow-700
      disabled:opacity-50 disabled:hover:bg-yellow-500 disabled:active:bg-yellow-500
    `,
    danger: `
      bg-red-500 text-white
      hover:bg-red-600 active:bg-red-700
      disabled:opacity-50 disabled:hover:bg-red-500 disabled:active:bg-red-500
    `,
  }

  return [base, colors[props.color ?? 'success']]
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