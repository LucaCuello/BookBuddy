<template>
  <div class="book-container">
    <TransitionGroup name="fade">
      <template v-for="(book, index) in bookList" :key="book.name">
        <div class="book" :class="book.color">
          <div class="book-title-container">
            <Transition name="pop">
              <template v-if="book.read">
                <div class="check-container">
                  <v-icon name="bi-check-lg" class="check-btn" />
                </div>
              </template>
              <template v-else>
                <img :src="logo" draggable="false" alt="book logo" />
              </template>
            </Transition>
            <span class="book-name">{{ book.name }}</span>
          </div>
          <template v-if="book.read">
            <v-icon
              name="bi-bookmark-check-fill"
              class="read-btn read-filled"
              @click="markAsRead(index)"
            />
          </template>
          <template v-else>
            <v-icon name="bi-bookmark-check" class="read-btn" @click="markAsRead(index)" />
          </template>
          <v-icon name="bi-trash" class="delete-btn" @click="removeBook(book)" />
          <p class="book-description">{{ book.description }}</p>
          <div class="book-author-container">
            <div class="line"></div>
            <span class="book-author">{{ book.author }}</span>
          </div>
        </div>
      </template>
    </TransitionGroup>
  </div>
</template>

<script lang="ts" setup>
import { type bookObject } from '../types/Types'
import logo from '@/assets/logo.svg'

const { bookList, removeBook, markAsRead } = defineProps<{
  bookList: bookObject[]
  removeBook: (selectedBook: {}) => void
  markAsRead: (index: number) => void
}>()
</script>

<style scoped>
.book-container {
  display: flex;
  justify-content: center;
  align-items: center;
  align-self: center;
  gap: 30px;
  width: 100%;
  max-width: 1000px;
  flex-wrap: wrap;
}
.book {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-direction: column;
  position: relative;
  overflow: hidden;
  width: 50px;
  height: 400px;
  gap: 15px;
  border-radius: 15px 15px 0px 0px;
  padding: 15px 0;
  transition: all 200ms ease;
}

.book:hover {
  width: 250px;
  transition: all 200ms ease;
}

.delete-btn,
.read-btn {
  position: absolute;
  cursor: pointer;
  opacity: 0;
  right: 15px;
  color: var(--main-color);
  transition: all 200ms ease;
}

.check-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: rgba(108, 151, 108, 0.144);
}
.check-btn {
  color: rgb(108, 151, 108);
  z-index: 1;
}

.read-btn {
  right: unset;
  left: 15px;
}

.read-btn:hover {
  transform: scale(1.2);
  transition: all 200ms ease;
}

.delete-btn:hover {
  transform: scale(1.2);
  transition: all 200ms ease;
}
.book-title-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 15px;
  width: 100%;
}

.book-author-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: 25px;
  width: 100%;
}

.book span {
  color: #00000094;
  writing-mode: vertical-rl;
  text-orientation: mixed;
  text-align: center;
}

.book-name {
  font-family: var(--avenir-font-book);
  font-weight: 800;
  font-size: 15px;
}

.book-author {
  font-family: var(--minion-font-italic);
  font-weight: 800;
  font-size: 12px;
  text-transform: uppercase;
}

.book-description {
  position: absolute;
  user-select: none;
  width: 100%;
  padding: 0px 10px;
  font-size: 12px;
  opacity: 0;
}
.book img {
  width: 40px;
  height: auto;
}
.line {
  width: 100%;
  height: 1px;
  background-color: #00000041;
}

.book:hover .delete-btn,
.book:hover .read-btn {
  opacity: 1;
}

.book:hover span {
  writing-mode: horizontal-tb;
  transition: all 600ms ease-in;
  animation: textOpacity 1s forwards;
}

.book:hover p {
  position: static;
  opacity: 1;
  transition: all 1s ease-in-out;
}

/* Animations */

@keyframes textOpacity {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-to {
  opacity: 1;
}

.pop-enter-from,
.pop-leave-to {
  transform: scale(0);
}

.pop-enter-to {
  transform: scale(1);
}

.pop-enter-active {
  transition: transform 200ms ease;
}

.fade-enter-active {
  transition: opacity 300ms ease;
}

.fade-move {
  transition: all 200ms ease-out;
}

.fade-leave-active {
  position: absolute;
  transition: all 300ms ease-out;
}
</style>
