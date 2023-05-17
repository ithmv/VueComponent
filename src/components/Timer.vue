<template>
  <div class="timer" @click="startTimer" :class="{ 'disabled': isDisabled }">
    <a href="https://www.google.com" target="_blank" @click="handleClick">{{ buttonText }}</a>
    <span :class="{ 'show-padding': timerText !== '' }">{{ timerText }}</span>
  </div>
</template>
  
  <style lang="scss">
    .timer {
      display: flex;
      text-align: center;
      align-items: center;
      justify-content: center;
      background: var(--primary-color);
      border-radius: 10px;
      cursor: pointer;
      transition: border-radius 0.5s;
      max-height: 56px;
    
      a {
        font-family: 'Inter', sans-serif;
        font-weight: 400;
        font-size: 20px;
        padding: 16px 0px 16px 24px;
        margin: 0;
        color: #FAFAFA;
        text-decoration: none;
      }
      
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
    
      .no-pointer {
        cursor: default;
      }
    }
  </style>
  
  <script>
  export default {
    data() {
      return {
        countdown: 10,
        buttonText: 'Отправить сообщение',
        timerText: '',
        isDisabled: false,
        isFinished: false,
        interval: null
      };
    },
    computed: {
      formattedTime() {
        const minutes = Math.floor(this.countdown / 60);
        const seconds = this.countdown % 60;
        const formattedMinutes = String(minutes).padStart(2, '0');
        const formattedSeconds = String(seconds).padStart(2, '0');
        return `${formattedMinutes}:${formattedSeconds}`;
      },
    },
    methods: {
      startTimer() {
        if (this.isDisabled || this.isFinished) {
          return;
        }
  
        this.buttonText = 'Отправить повторно ';
        this.timerText = this.formattedTime;
        this.isDisabled = true;
  
        this.interval = setInterval(() => {
          this.countdown--;
  
          if (this.countdown <= 0) {
            clearInterval(this.interval);
            this.buttonText = 'Отправить повторно';
            this.timerText = '';
            this.isDisabled = false;
            this.isFinished = true;
          } else {
            this.buttonText = 'Отправить повторно ';
            this.timerText = this.formattedTime;
          }
        }, 1000);
      },
      handleClick(event) {
        if (this.isDisabled) {
          event.preventDefault();
          return false;
        }
  
        if (this.isFinished) {
          this.isFinished = false;
          this.countdown = 10;
          this.startTimer();
        }
      },
    },
  };
  </script>