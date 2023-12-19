<script setup>
import Card from '@/components/Card.vue'
import { inject } from 'vue'

defineProps({
  cards: Array,
})

const emit = defineEmits([
  'handleCardLike',
  'handleCardClick',
  'handleCardDelete',
  'handleEditAvatar',
  'handleEditProfile',
  'handleAddCard',
])

const currentUser = inject('currentUser')
</script>

<template>
  <main>
    <section
      class="mx-auto mt-0 flex max-w-[282px] flex-col flex-wrap items-center justify-start pt-[42px] md:max-w-[880px] md:flex-row md:items-start md:pt-[40px]"
      aria-label="Профиль пользователя"
    >
      <img
        class="relative mb-[26px] h-[120px] w-[120px] rounded-full object-cover md:mb-0 md:mr-[30px]"
        :src="currentUser?.avatar"
        alt="Аватар пользователя"
      />
      <button
        type="button"
        class="edit-button-bg absolute mb-[26px] box-border h-[120px] w-[120px] cursor-pointer rounded-full border-none px-[6px] py-[1px] opacity-0 hover:opacity-100 hover:transition hover:duration-300 hover:ease-in md:mb-0 md:mr-[30px]"
        aria-label="Редактировать профиль"
        @click="emit('handleEditAvatar')"
      />
      <div
        class="mr-0 flex min-w-[228px] flex-col justify-center text-center md:mr-auto md:max-w-[336px] md:justify-start md:text-left"
      >
        <div
          class="flex min-w-[282px] items-center justify-center p-0 md:min-w-[336px] md:justify-between md:pt-[22px]"
        >
          <h1
            class="m-0 max-w-[189px] overflow-hidden overflow-ellipsis whitespace-nowrap pr-[10px] text-[27px] font-medium leading-[33px] md:max-w-[294px] md:pr-[18px] md:text-[42px] md:leading-[48px]"
          >
            {{ currentUser?.name }}
          </h1>
          <button
            class="opacity-link profile-edit-button-bg box-border inline-block h-[18px] w-[18px] border-none border-black px-[6px] py-[1px] text-black md:h-[24px] md:w-[24px]"
            type="button"
            aria-label="Редактировать профиль"
            @click="emit('handleEditProfile')"
          />
        </div>
        <p
          class="m-0 w-full max-w-[282px] overflow-hidden overflow-ellipsis whitespace-nowrap pt-[7px] text-sm font-normal leading-[17px] md:max-w-[336px] md:text-lg md:leading-[22px]"
        >
          {{ currentUser?.about }}
        </p>
      </div>
      <button
        class="opacity-link profile-add-btn-bg mt-[33px] box-border h-[50px] w-full rounded-[2px] border-2 border-solid border-white px-[6px] py-[1px] outline-none md:mt-[35px] md:w-[150px]"
        type="button"
        aria-label="Добавить картинку"
        @click="emit('handleAddCard')"
      />
    </section>
    <section
      class="mx-auto my-0 max-w-[282px] pt-9 md:max-w-[581px] lg:max-w-[880px] lg:pt-[51px]"
      aria-label="Фотографии"
    >
      <ul
        class="m-0 grid list-none grid-cols-1 gap-x-[17px] gap-y-[20px] p-0 md:grid-cols-2 lg:grid-cols-3"
      >
        <Card
          v-for="card in cards"
          :key="card._id"
          :card="card"
          :onLikeClick="() => emit('handleCardLike', card)"
          :onCardClick="() => emit('handleCardClick', card)"
          :onDeleteClick="() => emit('handleCardDelete', card)"
        />
      </ul>
    </section>
  </main>
</template>

<style scoped>
.edit-button-bg {
  background: rgba(0, 0, 0, 0.8) url(/images/profileEdit.svg) no-repeat center;
}

.profile-edit-button-bg {
  background: transparent url(/images/EditButton.svg) no-repeat center/contain;
  outline: none;
}

.profile-add-btn-bg {
  background: transparent url(/images/plus.svg) no-repeat center;
}

@media screen and (max-width: 767px) {
  .profile-add-btn-bg {
    background: transparent url(/images/plus_small.svg) no-repeat center;
  }
}
</style>
