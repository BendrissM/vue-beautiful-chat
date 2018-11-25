<template>
  <div class="user-list">
    <transition name="fadeDown">
      <div v-if="show" class="togglebutton">
        <div class="switch"><span class="text-bold-500">Set Username</span>
          <form @submit.prevent="sendUsername()" style="display:unset;" class="chat-app-input row" autocomplete="off">
            <fieldset class="form-group position-relative has-icon-left col-lg-10 col-8 m-0">
            <div class="form-control-position">
                <i class="ft-user"></i>
            </div>
            <input v-model="name" class="form-control" id="iconLeft4" placeholder="Type your message" type="text">
            
            </fieldset>
            <br>
            <fieldset class="form-group position-relative has-icon-left col-lg-2 col-4 m-0">
            <button type="submit" class="btn btn-raised btn-primary">
                <i class="fas fa-paper-plane hidden-lg-up"></i> Send</button>
            </fieldset>
        </form>
        </div>
        <p>you can set the username if you want.</p>
      </div>
    </transition>
    <transition name="fadeDown">
      <div v-if='showS' class="togglebutton">
        <div class="alert alert-success mb-2" role="alert">
            <strong>Well done! {{ name }}</strong> You successfully set the username.
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
export default {
  props: {
    participants: {
      type: Array,
      required: true
    },
    show: {
      type: Boolean,
      required: true
    },
    showS: {
      type: Boolean,
      required: true
    },
    socketId: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      name: ''
    }
  },
  methods: {
    sendUsername() {
      axios
        .put('/setName', {
          id: this.socketId,
          name: this.name
        })
        .then(() => {
          if (this.name.length === 0) {
            return
          }
          this.show = false
          setTimeout(() => {
            this.showS = true
          }, 1000)
          let data = {
            name: this.name,
            id: this.socketId
          }
          this.$socket.emit('username', data)
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>
<style scoped>
.user-list {
  height: 100%;
  overflow: auto;
  padding-left: 5px;
  padding-top: 8px;
}
.img-msg {
  border-radius: 50%;
  width: 50px;
  margin-right: 5px;
}
</style>
