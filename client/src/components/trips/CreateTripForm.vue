<template>
  <ValidationObserver v-slot="{ invalid }">
    <form class="form-styling" v-if="!message" @submit.prevent="createTrip">
      <!-- Trip name -->
      <ValidationProvider name="name" rules="required|alpha_spaces" v-slot="{ errors }">
        <div class="form-group">
          <label for="name">Title</label>
          <input name="name" type="text" class="form-control" placeholder="name" v-model="trip.name"/>
          <div  v-if="errors[0]" class="alert-danger">{{ errors[0] }}</div>
        </div>
      </ValidationProvider>
      <ValidationProvider name="status"  v-slot="{ errors }">
        <div class="form-group">
          <label for="status">Public</label>
          <input name="status" type="checkbox" class="form-control" placeholder="name" v-model="trip.public"/>
          <div  v-if="errors[0]" class="alert-danger">{{ errors[0] }}</div>
        </div>
      </ValidationProvider>
      <!-- Button -->
      <input type="submit" :disabled="invalid" value="Submit" class="button-styling"/>
      <div v-if="message">
        <div class="alert alert-danger">{{message}}</div>
      </div>
    </form>
    <div v-if="message">
      <div class="card-header">{{message}}</div>
      <b-button class="button-styling" type=reset @click="resetData() ">Add Another Trip</b-button>
    </div>
    <hr/>
  </ValidationObserver>
</template>

<script>
export default {
  name: 'CreateTripForm',
  data() {
    return {
      trip: {},
      message: ''
    }
  },
  methods: {
    resetData() {
      this.message = ''
      this.trip = {}
    },
    createTrip() {
      this.trip.user = this.$store.state.account.user._id
      var payload = {}
      payload.user = this.$store.state.account.user
      payload.trip = this.trip
      this.$store.dispatch('account/addTrip', payload).then(
        () => {
          this.message = 'Your trip has been added'
          this.trip = {}
        },
        error => {
          this.message = error.response.data.message
        }
      ).catch(error => {
        this.message = 'Your request could not be prosed at this time'
        console.log(error.toString())
      })
    }
  }
}
</script>

<style scoped>

</style>
