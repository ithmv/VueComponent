<template>
  <div class="btns">

    <a  class="pass" href="https://www.google.com" target="_blank">Не помню пароль</a>

    <div class="btns__timer" @click="startTimer" :class="{ 'disabled': isDisabled }">
      <a href="https://www.google.com" target="_blank" @click="handleClick">{{ buttonText }}</a>
      <span :class="{ 'show-padding': timerText !== '' }">{{ timerText }}</span>
    </div>

    <div class="btns__timer off"  >
      <a href="https://www.google.com" target="_blank" @click="handleClick">Отправить сообщение</a>
      <span></span>
    </div>

    <div class="btns__icon">
      <div class="btns__icon_item">
        <img src="../assets/cross.svg" width="12" alt="">
      </div>
      <div class="btns__icon_item big">
        <img src="../assets/cross.svg" width="20" alt="">
      </div>

      <div class="btns__icon_item">
        <img src="../assets/ArrLeft.svg" width="6" alt="">
      </div>
      <div class="btns__icon_item big">
        <img src="../assets/ArrLeft.svg" width="10" alt="">
      </div>
      <div class="btns__icon_item">
        <img src="../assets/ArrRight.svg" width="6" alt="">
      </div>
      <div class="btns__icon_item big">
        <img src="../assets/ArrRight.svg" width="10" alt="">
      </div>
      <div class="btns__icon_item">
        <img src="../assets/Like.svg" width="12" alt="">
      </div>
      <div class="btns__icon_item big">
        <img src="../assets/Like.svg" width="20" alt="">
      </div>

    </div>

    <div class="btns__arr">
      <a href="https://www.google.com" target="_blank">Отправить сообщение</a>
      <span style="background: none;"><img src="../assets/ArrRight.svg" width="10" height="20" alt=""></span>
    </div>
    
    <div class="btns__arr off"  >
      <a href="https://www.google.com" target="_blank" >Отправить сообщение</a>
      <span style="background: none;"><img src="../assets/ArrRight.svg" width="10" height="20" alt=""></span>
    </div>

    <div class="btns__arr">
      <span class="left" style="background: none;"><img src="../assets/ArrLeft.svg" width="10" height="20" alt=""></span>
      <a style="padding: 16px 0;" href="https://www.google.com" target="_blank">Отправить сообщение</a>
    </div>
    
    <div class="btns__arr off"  >
      <span class="left" style="background: none;"><img src="../assets/ArrLeft.svg" width="10" height="20" alt=""></span>
      <a style="padding: 16px 0;" href="https://www.google.com" target="_blank" >Отправить сообщение</a>
    </div>

  </div>
</template>

<script>
import { ref, reactive, watch } from 'vue';

export default {
  setup() {
    const countdown = ref(10);
    const buttonText = ref('Отправить сообщение');
    const timerText = ref('');
    const isDisabled = ref(false);
    const isFinished = ref(false);
    let interval = null;

    const formattedTime = () => {
      const minutes = Math.floor(countdown.value / 60);
      const seconds = countdown.value % 60;
      const formattedMinutes = String(minutes).padStart(2, '0');
      const formattedSeconds = String(seconds).padStart(2, '0');
      return `${formattedMinutes}:${formattedSeconds}`;
    };

    const startTimer = () => {
      if (isDisabled.value || isFinished.value) {
        return;
      }

      buttonText.value = 'Отправить повторно ';
      timerText.value = formattedTime();
      isDisabled.value = true;

      interval = setInterval(() => {
        countdown.value--;

        if (countdown.value <= 0) {
          clearInterval(interval);
          buttonText.value = 'Отправить повторно';
          timerText.value = '';
          isDisabled.value = false;
          isFinished.value = true;
        } else {
          buttonText.value = 'Отправить повторно ';
          timerText.value = formattedTime();
        }
      }, 1000);
    };

    const handleClick = (event) => {
      if (isDisabled.value) {
        event.preventDefault();
        return false;
      }

      if (isFinished.value) {
        isFinished.value = false;
        countdown.value = 10;
        startTimer();
      }
    };

    return {
      countdown,
      buttonText,
      timerText,
      isDisabled,
      isFinished,
      startTimer,
      handleClick
    };
  },
};
</script>

<style lang="scss">

  .btns {
    display: flex;
    flex-direction: column;
    gap: 30px;

    .pass {
    color: #FF6E00;
    margin: 0;
    padding: 0;
    width: fit-content;

     &:hover {
      text-shadow: var(--shadow-color);
      background: linear-gradient(90deg, #D7650F 0%, #E1250A 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
     }

     &:active {
      background: linear-gradient(90deg, #853900 0%, #AA1500 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
     }
    
  }

    &__timer, &__arr {
      display: flex;
      text-align: center;
      align-items: center;
      justify-content: center;
      background: var(--primary-color);
      border-radius: 10px;
      cursor: pointer;
      transition: border-radius 0.5s;
      max-height: 56px;
      max-width: 304px;

      span {
        padding: 0;
        margin: 20px 28px 20px 8px;
        border-radius: 5px;
        background: #C4250E;
        font-size: 13px;
        color: #FAFAFA;

        &.show-padding {
          padding: 4px;
        }
      }

      &:hover {
        border-radius: 20px !important;
        box-shadow: 0px 8px 24px rgba(215, 101, 15, 0.7);
      }

      &:active {
        background: var(--click-color);
        border-radius: 10px;
      }

      &.disabled {
        cursor: not-allowed;

        a {
          pointer-events: none;
        }
      }

      &.off {
        cursor: not-allowed;
        background: var(--disable-color);

        a {
          pointer-events: none;
          opacity: .5;
        }

        &:hover {
          box-shadow: none !important;
          border-radius: 10px !important;

        }
      }

      .no-pointer {
        cursor: default;
      }
    }

    &__icon {
      display: flex;
      gap: 10px;

      &_item {
        cursor: pointer;
        width: 36px;
        height: 36px;
        background: var(--primary-color);
        border-radius: 10px;
        display: flex;
        align-items: center;
        justify-content: center;

        &:hover {
          box-shadow: var(--shadow-color);
          transition: .3s;
          border-radius: 40px;
        }

        &:active {
          background: var(--click-color);
        }

        &.big {
          width: 52px;
          height: 52px;
        }
      }
    }

    &__arr {
      a {
        margin-right: 12px;
      }

      &.off {
        cursor: not-allowed;
        background: var(--disable-color);

        a {
          pointer-events: none;
          opacity: .5;
          margin-right: 12px;
        }

        &:hover {
          box-shadow: none !important;
          border-radius: 10px !important;

        }
      }
    }
  }



</style>