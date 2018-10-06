<template>
  <div class="notificationsWrap">
    <div class="notificationBox">

      <div @click="showNotifications = !showNotifications" class="bellButton">
        <div class="redCircle"><p v-text="notificationCount"></p></div>
        <i class="fas fa-bell"></i>
      </div>

      <transition name="appear">
        <div v-if="showNotifications" class="notificationHeader">
            <h1>NOTIFICATIONS</h1>
            <div class="triangle"></div>
        </div>
      </transition>

       <transition name="appear2">
        <div v-if="showNotifications" class="notificationBody">
            <p v-if="notifications === undefined || notifications.length === 0">
                No notifications to display.
            </p>
            <single-notification
            :key="notification.id"
            v-for="notification in notifications"
            :notification ='notification'
            v-on:removeNotification="removeNotification(notification)"
            />
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
    import singleNotification from './SingleNotification'
    export default {
  name: 'NotificationBox',
  components: {
      singleNotification
  },
  data(){
      return{
          notifications: [],
          showNotifications: true
      }
  },
  computed:{
      //Counting Notifications thate are not of type 'bonus'
      notificationCount(){
          let count = 0
          this.notifications.forEach(function (notification) {
              if (notification.type !== 'bonus'){
                  count +=1
              }
          })
          return count
      }
  },
  mounted(){
      //On load pull data from api
      this.axios.get('https://api.myjson.com/bins/jwk38')
          .then((response) => {
              this.notifications = response.data
          })
  },
// TO CONSTANTLY CHECK IF THE API CHANGED AND ALWAYS LOAD THE DATA FROM THE API
// updated(){
//     this.axios.get('https://api.myjson.com/bins/jwk38')
//         .then((response) => {
//             this.notifications = response.data
//         })
// },
  methods:{
      //Removes a notification object from the notifications array
      removeNotification(notification){
          let index = this.notifications.indexOf(notification);
          this.notifications.splice(index, 1);
      }
  }
}
</script>

<style lang="scss" scoped>
  .notificationsWrap {
    width: 350px;
    height: 500px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .notificationBox {
      overflow: hidden;
      .bellButton{
        float: right;
        margin-right: 40px;
        margin-bottom: 20px;
        text-align: center;
          &:hover{
              cursor: pointer;
              i{
                  background: darken(#fff, 15);
              }
          }
        i{
          font-size: 30px;
          color: #999999;
          background: #fff;
          padding: 5px 7px;
          border-radius: 5px;
        }
        .redCircle{
          background: #E54D2D;
          min-width: 20px;
          height: 20px;
          border-radius: 200px;
          margin-left: 25px;
          position: relative;
          left: 10px;
          top: 5px;
        }
      }
      .notificationHeader {
        width: 100%;
        background: #6A98EE;
        border-top-left-radius: 5px;
        border-top-right-radius: 5px;
        display: flex;
        align-items: center;
        -webkit-box-shadow: 2px 2px 11px -2px rgba(0,0,0,0.75);
        -moz-box-shadow: 2px 2px 11px -2px rgba(0,0,0,0.75);
        box-shadow: 2px 2px 11px -2px rgba(0,0,0,0.75);
        .triangle {
          width: 40px;
          height: 40px;
          background: #6A98EE;
          display: inline-block;
          position: relative;
          left: 125px;
          bottom: 10px;
          transform: rotate(45deg);
        }
        h1 {
          margin: 0;
          color: #fff;
          font-weight: 400;
          font-size: 15px;
          padding: 15px;
          display: inline-block;
        }
      }
      .notificationBody{
        width: 100%;
        height: 320px;
        background: #fff;
        overflow: scroll;
        -ms-overflow-style: none;  // IE 10+ remove scrollbar
        overflow: -moz-scrollbars-none;  // Firefox remove scrollbar
        border-bottom-left-radius: 5px;
        border-bottom-right-radius: 5px;
        -webkit-box-shadow: 2px 22px 11px -2px rgba(0,0,0,0.75);
        -moz-box-shadow: 2px 22px 11px -2px rgba(0,0,0,0.75);
        box-shadow: 2px 2px 11px -2px rgba(0,0,0,0.75);
        &::-webkit-scrollbar {
          display: none;  // Remove sidebar for Safari and Chrome
        }
          p{
              text-align: center;
              margin-top: 30px;
              color: #929292;
          }
      }
    }
  }
.appear-enter-active, .appear-leave-active{
    transition: all 0.5s;
}
.appear-enter, .appear-leave-to{
    opacity: 0;
    transform: scale(0);
}
.appear2-enter-active, .appear2-leave-active{
  transition: all 0.5s;
}
.appear2-enter, .appear2-leave-to{
  opacity: 0;
  margin-top: 1000px;
}
</style>
