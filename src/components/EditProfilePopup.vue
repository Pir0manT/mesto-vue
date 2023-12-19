<script setup>
import { computed, inject, ref, watch } from 'vue'
import PopupWithForm from '@/components/PopupWithForm.vue'

const props = defineProps({
  isOpen: Boolean,
})

const emit = defineEmits(['editProfile'])
const currentUser = inject('currentUser')
const isOpen = computed(() => props.isOpen)

const name = ref('')
const about = ref('')

const handleSubmit = () => {
  emit('editProfile', { name: name.value, about: about.value })
}

watch([currentUser, isOpen], () => {
  if (currentUser?.value.name) {
    name.value = currentUser.value.name
  }
  if (currentUser?.value.about) {
    about.value = currentUser.value.about
  }
})
</script>

<template>
  <PopupWithForm
    :is-open="isOpen"
    :name="'profile'"
    :title="'Редактировать профиль'"
    :btn-title="'Сохранить'"
    @submit="handleSubmit"
  >
    <input
      v-model="name"
      class="popup_input"
      type="text"
      placeholder="Ваше имя"
      id="name-input"
      minLength="2"
      maxLength="40"
      name="name"
      required
    />
    <span class="popup_error"></span>
    <input
      v-model="about"
      class="popup_input"
      required
      type="text"
      placeholder="Немного о себе"
      id="description-input"
      minLength="2"
      maxLength="200"
      name="about"
    />
    <span class="popup_error"></span>
  </PopupWithForm>
</template>

<style scoped></style>
