<script setup>
import { computed, ref, watch } from 'vue'
import PopupWithForm from '@/components/PopupWithForm.vue'

const props = defineProps({
  isOpen: Boolean,
})
const emit = defineEmits(['addCard'])
const isOpen = computed(() => props.isOpen)
const name = ref('')
const link = ref('')

const handleSubmit = () => {
  emit('addCard', { name: name.value, link: link.value })
}

watch(isOpen, () => {
  name.value = ''
  link.value = ''
})
</script>

<template>
  <PopupWithForm
    :is-open="isOpen"
    :name="'add-card'"
    :title="'Новое место'"
    :btn-title="'Создать'"
    @submit="handleSubmit"
  >
    <input
      v-model="name"
      class="popup_input"
      type="text"
      placeholder="Название"
      id="card-name"
      minLength="2"
      maxLength="30"
      name="name"
      required
    />
    <span class="popup_error"></span>
    <input
      v-model="link"
      class="popup_input"
      type="url"
      name="link"
      id="element-link"
      placeholder="Ссылка на картинку"
      required
    />
    <span class="popup_error"></span>
  </PopupWithForm>
</template>
