<script setup>
import { computed, inject } from 'vue'

const props = defineProps({
  card: Object,
  onLikeClick: Function,
  onCardClick: Function,
  onDeleteClick: Function,
})

const currentUser = inject('currentUser')

const name = computed(() => props.card.name)
const link = computed(() => props.card.link)
const owner = computed(() => props.card.owner)
const likes = computed(() => props.card.likes)
const itsMyCard = computed(() => owner.value._id === currentUser.value._id)
const isLiked = computed(() => likes.value.some((like) => like._id === currentUser.value._id))
</script>

<template>
  <li class="relative m-0 max-w-[282px] rounded-[10px] bg-white p-0">
    <button
      v-if="itsMyCard"
      type="button"
      aria-label="Удалить"
      @click="onDeleteClick"
      class="del-btn-bg absolute ml-[244px] mt-5 h-[19px] w-[18px] border-none p-0"
    />
    <img
      :src="link"
      :alt="name"
      class="object-position-center-0 m-0 mb-1 h-[282px] w-full rounded-b-[0] rounded-t-[10px] object-cover object-[center_0] p-0"
      @click="onCardClick"
    />
    <div class="box-border flex min-h-[79px] items-center justify-between pl-[22px] pr-5">
      <h2
        class="m-w-[216px] m-0 overflow-hidden overflow-ellipsis whitespace-nowrap text-2xl font-black leading-[29px] text-black"
      >
        {{ name }}
      </h2>
      <div class="flex h-[35px] w-[22px] flex-col items-center">
        <button
          type="button"
          @click="onLikeClick"
          aria-label="Добавить в избранное"
          :class="{ 'heart-liked-btn-bg': isLiked, 'heart-btn-bg': !isLiked }"
          class="h-[19px] w-[22px] border-none p-0 shadow-none outline-none hover:cursor-pointer hover:opacity-50"
        />
        <div class="pt-[1px] text-[13px] font-normal leading-[16px] text-black">
          {{ likes.length }}
        </div>
      </div>
    </div>
  </li>
</template>

<style scoped>
.del-btn-bg {
  background: transparent url(/images/DeleteButton.svg) no-repeat center/contain;
}

.heart-btn-bg {
  background: #fff url(/images/heart.svg) no-repeat center;
}

.heart-liked-btn-bg {
  background: #fff url(/images/heart_active.svg) no-repeat center;
  opacity: 1;
}
</style>
