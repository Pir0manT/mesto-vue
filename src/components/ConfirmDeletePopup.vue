<script setup>
import PopupWithForm from '@/components/PopupWithForm.vue'

defineProps({
  isOpen: Boolean,
})
const emit = defineEmits(['deleteCard'])
</script>

<template>
  <PopupWithForm
    :isOpen="isOpen"
    :name="'confirm'"
    :title="'Вы уверены?'"
    :btnTitle="'Да'"
    @submit="emit('deleteCard')"
  />
</template>
