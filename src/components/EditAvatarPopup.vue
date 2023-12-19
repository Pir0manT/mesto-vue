<script setup>
import PopupWithForm from '@/components/PopupWithForm.vue'
import { computed, inject, ref, watch } from 'vue'

const props = defineProps({
  isOpen: Boolean,
})

const emit = defineEmits(['editAvatar'])
const currentUser = inject('currentUser')
const isOpen = computed(() => props.isOpen)

const avatarLink = ref('')

const handleSubmit = () => {
  emit('editAvatar', { avatar: avatarLink.value })
}

watch(
  [currentUser, isOpen],
  () => {
    if (currentUser?.value.avatar) {
      avatarLink.value = currentUser.value.avatar
    }
  },
  { deep: true }
)
</script>

<template>
  <PopupWithForm
    :is-open="isOpen"
    :name="'avatar'"
    :title="'Обновить аватар'"
    :btn-title="'Сохранить'"
    @submit="handleSubmit"
  >
    <input
      v-model="avatarLink"
      class="popup_input"
      type="url"
      name="link"
      placeholder="Ссылка на аватар"
      required
    />
    <span class="popup_error"></span>
  </PopupWithForm>
</template>
