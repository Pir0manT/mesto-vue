<script setup>
import { inject, ref, watch } from 'vue'

const props = defineProps({
  name: String,
  isOpen: Boolean,
  title: String,
  btnTitle: String,
  isConfirmPopup: Boolean,
})

const emit = defineEmits(['submit'])

const submitBtnRef = ref(null)

const { isSaving, loadingText, closeAllPopups } = inject('popupState')

watch(
  () => isSaving,
  () => {
    submitBtnRef.value.textContent = isSaving ? loadingText : props.btnTitle
  }
)

const onSubmit = () => {
  emit('submit')
}
</script>

<template>
  <div
    :class="{ 'visible opacity-100': props.isOpen, 'invisible opacity-0': !props.isOpen }"
    class="popup"
    @click.self="closeAllPopups"
  >
    <div
      :class="{ popup_container: !isConfirmPopup, popup_delete: isConfirmPopup }"
      class="relative w-full max-w-[282px] rounded-[10px] bg-white md:max-w-[430px]"
    >
      <h2 class="popup_title">
        {{ title }}
      </h2>
      <form
        :name="name"
        @submit.prevent="onSubmit"
        class="flex max-w-[238px] flex-col border-none p-[25px] pt-[75px] md:max-w-[358px] md:px-9 md:pb-9 md:pt-[54px]"
      >
        <slot></slot>
        <button ref="submitBtnRef" class="popup_save" type="submit">
          {{ isSaving ? loadingText : btnTitle }}
        </button>
      </form>
      <button
        class="popup__close link-opacity"
        type="button"
        aria-label="закрыть"
        @click="closeAllPopups"
      />
    </div>
  </div>
</template>

<style scoped>
.popup_container {
  @apply max-h-[330px];
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.25);
}
.popup_delete {
  @apply min-h-[181px];
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.25);
}
.popup_title {
  @apply m-0 pl-[22px] pt-[25px] text-[18px] font-black leading-[22px] text-black md:pl-[36px] md:pt-[34px] md:text-2xl md:leading-[29px];
}

.popup_save {
  @apply mx-auto mb-0 mt-[10px] min-h-[46px] w-full rounded-[2px] border-none bg-black text-center text-[14px] font-normal leading-[17px] text-white hover:cursor-pointer hover:opacity-80 md:m-0 md:min-h-[50px] md:text-lg md:leading-[22px];
}
</style>
