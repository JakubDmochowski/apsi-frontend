<template>
  <div class="p-4">
    <b-card
      title="Create user"
    >
      <b-form @submit.prevent="handleSubmit">
        <b-form-group
          label-cols="3"
          label-align="right"
          label-class="font-weight-bold"
          label-for="Name"
          label="Name"
        >
          <b-form-input id="Name" required v-model="item.name" />
        </b-form-group>
        <b-form-group
          label-cols="3"
          label-align="right"
          label-class="font-weight-bold"
          label-for="Surname"
          label="Surname"
        >
          <b-form-input id="Surname" required v-model="item.surname" />
        </b-form-group>
        <b-form-group
          label-cols="3"
          label-align="right"
          label-class="font-weight-bold"
        >
          <b-form-checkbox id="generate_password" v-model="meta.generate_password">
            Generate password
          </b-form-checkbox>
        </b-form-group>
        <b-form-group
          v-if="!meta.generate_password"
          label-cols="3"
          label-align="right"
          label-class="font-weight-bold"
          label-for="Password"
          label="Password"
        >
          <b-form-input id="Password" type="password" required v-model="item.password" />
        </b-form-group>
        <b-form-group
          label-cols="3"
          label-align="right"
          label-class="font-weight-bold"
          label-for="Email"
          label="Email"
        >
          <b-form-input id="Email" type="email" v-model="item.email" />
        </b-form-group>
        <b-form-group
          label-cols="3"
          label-align="right"
          label-class="font-weight-bold"
          label-for="Phone"
          label="Phone"
        >
          <b-form-input id="Phone" type="tel" v-model="item.phone_number" />
        </b-form-group>
        <b-form-group
          label-cols="3"
          label-align="right"
          label-class="font-weight-bold"
          label-for="Birthdate"
          label="Birthdate"
        >
          <b-calendar v-model="item.birthdate" locale="en-US" />
        </b-form-group>
        <div class="d-flex justify-content-end">
          <b-button class="mr-2" type="submit" variant="success">
            Submit
          </b-button>
          <b-button @click="handleCancel" variant="danger">
            Cancel
          </b-button>
        </div>
      </b-form>
    </b-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    item: {
      name: '',
      surname: '',
      password: '',
      email: '',
      phone_number: '',
      birthdate: '',
    },
    meta: {
      generate_password: true,
    },
  }),
  async asyncData({ store, redirect }) {
    if(!store.state.user.is_staff) {
      redirect('/')
    }
  },
  methods: {
    handleSubmit() {
      if(this.meta.generate_password) {
        this.item.password = Math.random().toString(36).replace(/[^a-z]+/g, '').substr(0, 16)
      }
      this.$axios.post(
        'api/users/employees/',
        this.item
      )
      .then(() => {
        this.$router.push({ path: "/users" })
      })
    },
    handleCancel() {
      this.$router.back()
    }
  }
}
</script>

<style>

</style>