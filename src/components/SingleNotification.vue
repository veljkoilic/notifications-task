<template>
    <transition name="shrink">
      <div class="singleNotification">
          <div :class="{ promotion: notification.type === 'Promotion' }">
            <h3 v-text="notification.title" v-if="notification.type !== 'Promotion'"></h3>
            <p v-text="notification.text" v-if="notification.type === 'text'"></p>
            <p v-text="notification.requirement" v-else-if="notification.type === 'bonus'"></p>
            <img :src="notification.image" alt="" v-else>
              <a :href="notification.link" target="_blank" v-if="notification.type === 'Promotion'">
                  <h3 v-text="notification.title"></h3>
              </a>
          </div>
          <p @click="$emit('removeNotification')" class="xButton"><i class="fas fa-times"></i></p>
      </div>
    </transition>
</template>

<script>
export default {
  name: 'single-notification',
  props: {
    notification: Object
  },
    computed:{
      //Returns the notification.expires function in seconds
      expirySeconds(){
          return this.notification.expires * 1000
      }
    },
  mounted(){
  //Remove notification when it expires
      if (this.notification.expires) {
          setTimeout(() => {
              this.$emit('removeNotification')
          }, this.expirySeconds)
      }
  }
}
</script>

<style lang="scss" scoped>
  .singleNotification{
    margin: 0 auto;
    min-height: 100px;
    padding: 0 20px;
    border-bottom: 1px solid lightgray;
    display: grid;
    grid-template-columns: 100fr 1fr;
    background: #fff;
      &:hover{
          padding-left: 15px;
          border-left: 5px solid green;
          transition: 0.3s ease-in-out all;
          .xButton{
              color: #e54d2d;
          }
      }
      div{
          display: flex;
          align-items: flex-start;
          justify-content: center;
          flex-direction: column;
      }
      .promotion{
          flex-direction: row;
          justify-content: flex-start;
          align-items: center;
      }
      h3{
          font-size: 15px;
      }
      p{
          font-size: 13px;
      }
      .xButton{
          font-size: 15px;
          font-weight: bold;
          cursor: pointer;
          color: #000;
          display: flex;
          align-items: center;
      }
      img{
          width: 40px;
          height: 40px;
      }
      a{
          margin-left: 15px;
          text-decoration: none;
          color: #000;
          &:hover{
              color: green;
          }
      }
  }
  .shrink-enter-active, .shrink-leave-active{
      transition: all 0.5s;
  }
  .shrink-enter, .shrink-leave-to{
      opacity: 0;
      transform: scale(-0.1);
  }

</style>
