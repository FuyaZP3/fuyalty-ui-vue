<script setup lang="ts">
import { twMerge, twJoin } from 'tailwind-merge';
import { computed, useAttrs } from 'vue';

type Colors = 'primary' | 'secondary' | 'info' | 'success' | 'warning' | 'error'
type Variants = 'filled' | 'outlined' | 'tonal'
type Status = 'base' | 'hover' | 'active' | 'disabled'

interface Props {
  label?: string
  color?: Colors
  variant?: Variants
  disabled?: boolean
  size?: string
}

const props = defineProps<Props>()

const emit = defineEmits<{
  (e: 'click', event: MouseEvent): void
}>()

const attrs = useAttrs()

const colors: Record<NonNullable<Props['color']>, Record<NonNullable<Props['variant']>, Record<Status, string>>> = {
  primary: {
    filled: {
      base: 'bg-orange-500 text-white',
      hover: 'hover:bg-orange-600',
      active: 'active:bg-orange-700',
      disabled: 'bg-orange-500 text-white opacity-50'
    },
    outlined: {
      base: 'text-orange-500 outline-1',
      hover: 'hover:bg-orange-50',
      active: 'active:bg-orange-100',
      disabled: 'text-orange-500 opacity-50'
    },
    tonal: {
      base: 'bg-orange-100 text-orange-500',
      hover: 'hover:bg-orange-200',
      active: 'active:bg-orange-300',
      disabled: 'text-orange-500 opacity-50'
    },
  },
  secondary: {
    filled: {
      base: 'bg-gray-500 text-white',
      hover: 'hover:bg-gray-600',
      active: 'active:bg-gray-700',
      disabled: 'bg-gray-500 text-white opacity-50'
    },
    outlined: {
      base: 'text-gray-500 outline-1',
      hover: 'hover:bg-gray-50',
      active: 'active:bg-gray-100',
      disabled: 'text-gray-500 opacity-50'
    },
    tonal: {
      base: 'bg-gray-100 text-gray-500',
      hover: 'hover:bg-gray-200',
      active: 'active:bg-gray-300',
      disabled: 'text-gray-500 opacity-50'
    },
  },
  info: {
    filled: {
      base: 'bg-cyan-500 text-white',
      hover: 'hover:bg-cyan-600',
      active: 'active:bg-cyan-700',
      disabled: 'bg-cyan-500 text-white opacity-50'
    },
    outlined: {
      base: 'text-cyan-500 outline-1',
      hover: 'hover:bg-cyan-50',
      active: 'active:bg-cyan-100',
      disabled: 'text-cyan-500 opacity-50'
    },
    tonal: {
      base: 'bg-cyan-100 text-cyan-500',
      hover: 'hover:bg-cyan-200',
      active: 'active:bg-cyan-300',
      disabled: 'text-cyan-500 opacity-50'
    },
  },
  success: {
    filled: {
      base: 'bg-green-500 text-white',
      hover: 'hover:bg-green-600',
      active: 'active:bg-green-700',
      disabled: 'bg-green-500 text-white opacity-50'
    },
    outlined: {
      base: 'text-green-500 outline-1',
      hover: 'hover:bg-green-50',
      active: 'active:bg-green-100',
      disabled: 'text-green-500 opacity-50'
    },
    tonal: {
      base: 'bg-green-100 text-green-500',
      hover: 'hover:bg-green-200',
      active: 'active:bg-green-300',
      disabled: 'text-green-500 opacity-50'
    },
  },
  warning: {
    filled: {
      base: 'bg-yellow-500 text-white',
      hover: 'hover:bg-yellow-600',
      active: 'active:bg-yellow-700',
      disabled: 'bg-yellow-500 text-white opacity-50'
    },
    outlined: {
      base: 'text-yellow-500 outline-1',
      hover: 'hover:bg-yellow-50',
      active: 'active:bg-yellow-100',
      disabled: 'text-yellow-500 opacity-50'
    },
    tonal: {
      base: 'bg-yellow-100 text-yellow-500',
      hover: 'hover:bg-yellow-200',
      active: 'active:bg-yellow-300',
      disabled: 'text-yellow-500 opacity-50'
    },
  },
  error: {
    filled: {
      base: 'bg-red-500 text-white',
      hover: 'hover:bg-red-600',
      active: 'active:bg-red-700',
      disabled: 'bg-red-500 text-white opacity-50'
    },
    outlined: {
      base: 'text-red-500 outline-1',
      hover: 'hover:bg-red-50',
      active: 'active:bg-red-100',
      disabled: 'text-red-500 opacity-50'
    },
    tonal: {
      base: 'bg-red-100 text-red-500',
      hover: 'hover:bg-red-200',
      active: 'active:bg-red-300',
      disabled: 'text-red-500 opacity-50'
    },
  }
}

const sizes: Record<NonNullable<Props['size']>, string> = {
  sm: 'px-2 py-1 text-sm',
  md: 'px-4 py-2 text-base',
  lg: 'px-6 py-3 text-lg'
}

const handleClick = (e: MouseEvent) => {
  if(props.disabled) return
  
  console.log('click')
  emit('click', e)
}

const classes = computed(() => {
  const baseClasses = 'inline-flex justify-center align-middle items-center px-4 py-2 rounded-md'

  if(!props.color || !props.variant) return ''
  
  const map = colors[props.color][props.variant]

  return twMerge(
    baseClasses, 
    map.base, 
    sizes[props.size ?? 'md'],
    !props.disabled && map.active,  //if button is not disabled generate active class
    !props.disabled && map.hover, 
    props.disabled && map.disabled, //if button is disabled generate disabled class 
    attrs.class   //User classes override default classes
  )
})
</script>

<template>
  <button
    :class="classes"
    @click="handleClick"
  >
    <span>
      <slot>{{ label }}</slot>
    </span>
  </button>
</template>