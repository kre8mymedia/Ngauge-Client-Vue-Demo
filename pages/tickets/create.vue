<template>
  <div>
    <navbar-component></navbar-component>

    <h3 class="m-4">Tickets</h3>

    <div class="container mt-4">
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          id="input-group-1"
          label="Subject:"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            v-model="form.subject"
            type="text"
            required
            placeholder="Enter subject"
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Description:" label-for="input-2">
          <b-form-textarea
            id="textarea"
            v-model="form.body"
            placeholder="Enter something..."
            rows="3"
            max-rows="6"
          ></b-form-textarea>
        </b-form-group>

        <b-form-group id="input-group-3" label="Status:" label-for="input-3">
          <b-form-select
            id="input-3"
            v-model="form.status"
            :options="options"
            required
          ></b-form-select>
        </b-form-group>

        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
      <b-card class="mt-3" header="Form Data Result">
        <pre class="m-0">{{ form }}</pre>
      </b-card>
    </div>
  </div>
</template>

<script>
  import Navbar from '~/components/Navbar.vue'
  export default {
    components: {
      'navbar-component': Navbar
    },
    data() {
      return {
        form: {
          user_id: 1,
          subject: '',
          body: '',
          status: null,
        },
        options: [{ text: 'Select Option', value: null }, 'Open', 'Closed'],
        show: true
      }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault()
        fetch('http://ngauge.ml/api/v1/tickets', {
          method: 'POST', // or 'PUT'
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.form)
        })
        .then(res => res.json())
        .catch(error => console.log(error))
        alert('Message sent!');
        this.onReset(evt);
      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.subject = ''
        this.form.body = ''
        this.form.status = null
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }
</script>