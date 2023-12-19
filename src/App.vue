<script setup>
import Header from '@/components/Header.vue'
import Main from '@/components/Main.vue'
import Footer from '@/components/Footer.vue'
import api from '@/utils/api.js'
import { onMounted, provide, ref } from 'vue'
import ImagePopup from '@/components/ImagePopup.vue'
import ConfirmDeletePopup from '@/components/ConfirmDeletePopup.vue'
import EditAvatarPopup from '@/components/EditAvatarPopup.vue'
import EditProfilePopup from '@/components/EditProfilePopup.vue'
import AddCardPopup from '@/components/AddCardPopup.vue'

const currentUser = ref({})
const cards = ref([])
const selectedCard = ref({})
const isImageOpen = ref(false)
const isConfirmDeletePopupOpen = ref(false)
const isEditAvatarPopupOpen = ref(false)
const isEditProfilePopupOpen = ref(false)
const isAddCardPopupOpen = ref(false)
const isSaving = ref(false)
const loadingText = ref('Сохранение...')

onMounted(() => {
  Promise.all([api.getProfile(), api.getInitialCards()])
    .then(([userProfile, initialCards]) => {
      currentUser.value = userProfile
      cards.value = initialCards
    })
    .catch((error) => console.log(error))
})
const handleUpdateAvatar = (avatar) => {
  loadingText.value = 'Сохранение аватара...'
  isSaving.value = true
  api
    .setAvatar(avatar)
    .then((userInfo) => {
      currentUser.value = userInfo
      closeAllPopups()
    })
    .catch((error) => console.log(error))
    .finally(() => (isSaving.value = false))
}

const handleUpdateUser = (newUser) => {
  loadingText.value = 'Сохранение профиля...'
  isSaving.value = true
  api
    .setProfile(newUser)
    .then((user) => {
      currentUser.value = user
      closeAllPopups()
    })
    .catch((error) => console.log(error))
    .finally(() => (isSaving.value = false))
}

const handleCardLike = (card) => {
  const cardIsLiked = card.likes.some((like) => like._id === currentUser.value._id)
  api
    .changeLike(card._id, cardIsLiked)
    .then((newCard) => {
      cards.value = cards.value.map((item) => (item._id === card._id ? newCard : item))
    })
    .catch((error) => console.log(error))
}

const handleCardDelete = () => {
  loadingText.value = 'Удаление...'
  isSaving.value = true
  api
    .delCard(selectedCard.value._id)
    .then(() => {
      cards.value = cards.value.filter((card) => card._id !== selectedCard.value._id)
      selectedCard.value = {}
      closeAllPopups()
    })
    .catch((error) => console.log(error))
    .finally(() => (isSaving.value = false))
}

const handleAddCard = (card) => {
  loadingText.value = 'Добавление места...'
  isSaving.value = true
  api
    .addCard(card)
    .then((newCard) => {
      cards.value = [newCard, ...cards.value]
      closeAllPopups()
    })
    .catch((error) => console.log(error))
    .finally(() => (isSaving.value = false))
}

const handleCardClick = (card) => {
  selectedCard.value = card
  isImageOpen.value = true
}

const handleCardDeleteClick = (card) => {
  selectedCard.value = card
  isConfirmDeletePopupOpen.value = true
}

const handleEditAvatarClick = () => {
  isEditAvatarPopupOpen.value = true
}

const handleEditProfileClick = () => {
  isEditProfilePopupOpen.value = true
}

const handleAddCardClick = () => {
  isAddCardPopupOpen.value = true
}

const closeAllPopups = () => {
  isImageOpen.value = false
  isConfirmDeletePopupOpen.value = false
  isEditAvatarPopupOpen.value = false
  isEditProfilePopupOpen.value = false
  isAddCardPopupOpen.value = false
}

provide('currentUser', currentUser)
provide('popupState', { isSaving, loadingText, selectedCard, closeAllPopups })
</script>

<template>
  <Header />
  <Main
    :cards="cards"
    @handle-card-like="handleCardLike"
    @handle-card-click="handleCardClick"
    @handle-card-delete="handleCardDeleteClick"
    @handle-edit-avatar="handleEditAvatarClick"
    @handle-edit-profile="handleEditProfileClick"
    @handle-add-card="handleAddCardClick"
  />
  <Footer />
  <ImagePopup :isOpen="isImageOpen" />
  <ConfirmDeletePopup :is-open="isConfirmDeletePopupOpen" @delete-card="handleCardDelete" />
  <EditAvatarPopup :is-open="isEditAvatarPopupOpen" @edit-avatar="handleUpdateAvatar" />
  <EditProfilePopup :is-open="isEditProfilePopupOpen" @edit-profile="handleUpdateUser" />
  <AddCardPopup :is-open="isAddCardPopupOpen" @add-card="handleAddCard" />
</template>
