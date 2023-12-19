<script setup>
import { computed, inject } from 'vue'

const props = defineProps({
  isOpen: Boolean,
})
const { closeAllPopups, selectedCard } = inject('popupState')

const imageLink = computed(() => selectedCard.value.link)
const name = computed(() => selectedCard.value.name)
</script>

<template>
  <div
    :class="{ 'visible opacity-100': props.isOpen, 'invisible opacity-0': !props.isOpen }"
    class="popup"
    @click="closeAllPopups"
  >
    <div class="relative">
      <button class="popup__close opacity-link" type="button" @click="closeAllPopups" />
      <img
        :src="imageLink"
        :alt="name"
        class="block max-h-[75vh] max-w-[75vw]"
        @click="closeAllPopups"
      />
      <p class="m-0 pt-[10px] text-xs font-normal leading-[15px]">{{ name }}</p>
    </div>
  </div>
</template>
