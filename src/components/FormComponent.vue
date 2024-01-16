
<script setup>
  import {ref, computed, watch} from 'vue'

  let text = ref('')
  const userInput = ref('');
  const isTyping = ref(false);
  let timer;

  watch(() => text.value, (newValue, oldValue) => {
    if (newValue !== oldValue) {
      isTyping.value = true;
      clearTimeout(timer);
      timer = setTimeout(() => {
        isTyping.value = false;
      }, 1000);
    }
  });

  const shownError = computed(() => {
    return text.value.length > 1000
  })

  const symbolsAmount = computed(() => {
    return text.value.length
  })
  const clearInput = () => {
    text.value = ''
  }
</script>

<template>
  <div class="form">
    <h1 class="form__title">Текст перед полем ввода</h1>
    <div class="form__wrapper">
      <textarea class="form__textarea" :class="{'form--textarea-error': shownError}" v-model="text"></textarea>
      <div class="form__label">Название поля</div>
      <div class="animation-container" :class="{ 'visible': isTyping }">
        <div class="spinner"></div>
      </div>
    </div>

    <p class="form__bottom form--error" v-show="shownError">Ошибка</p>
    <p class="form__bottom form--symbols">{{symbolsAmount}}/1000</p>
    <p class="form__bottom form--clear" @click="clearInput">Очистить/1000</p>
  </div>
  
</template>

<style lang="scss" scoped>
//Normalize
body {
  box-sizing: border-box;
  font-family: 'Roboto', sans-serif;
}
textarea {
  padding: 0;
  border: 0;
  outline: none;
}
//Normalize

.form {
  &__wrapper {
    margin-top: 4px;
    margin-bottom: 8px;
    position: relative;
    width: 326px;
    height: 146px;
  }
  &__textarea {
    padding: 20px 4px 20px 12px;
    resize: none;

    width: 100%;
    height: 100%;
    background-color: #F0F0F0;
    border-radius: 8px;
  }
  &__title {
    color: #303030;
    font-weight: 500;
    font-size: 18px;
    line-height: 24px;
    letter-spacing: 0.15%;
    white-space: nowrap;
  }
  &__label {
    position: absolute;
    top: 4px;
    left: 12px;

    color: #878F97;
    font-weight: 400;
    font-size: 16px; 
    line-height: 22px;
    letter-spacing: 0.15%;
    transition: all .5s ease;
  }
  &__textarea:focus + .form__label {
    font-weight: 400px;
    font-size: 11px;
    line-height: 12px;
    letter-spacing: 0.4%;
  }
  &__bottom {
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
    letter-spacing: 0.25%;
  }
  &--textarea-error {
    background: #FBF0EF;
  }
  &--error {
    color: #D6675C;
  }
  &--symbols {
    color: #878F97;
  }
  &--clear {
    color: #00B6D0;
  }
}
.animation-container {
  position: absolute;
  top: 7px;
  right: 7px;
  display: flex;
  align-items: center;
  visibility: hidden;
  &.visible {
    visibility: visible;
  }
}

.spinner {
  border: 3px solid transparent;
  border-top: 3px solid #00B6D0;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>