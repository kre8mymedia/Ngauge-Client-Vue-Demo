<template>
  <div>
    <navbar-component></navbar-component>

    <h3 class="m-4">Tickets</h3>

    <hr>

    <div class="container-fluid mt-4">
      
      <div class="container">
        <div class="row">
          <div class="col-sm-6">
            <h4>Open Tickets</h4>
            <div v-for="open_ticket in open_tickets" class="card mb-2 p-2" :key="open_ticket.id">
              <b>{{ open_ticket.user_id }}</b>
              <p>{{ open_ticket.body }}</p>
              <small>{{ open_ticket.created_at }}</small>
            </div>  
          </div>

          <div class="col-sm-6 mb-5">
            <h4>Closed Tickets</h4>
            <div v-for="closed_ticket in closed_tickets" class="card mb-2 p-2" :key="closed_ticket.id">
              <b>{{ closed_ticket.user_id }}</b>
              <p>{{ closed_ticket.body }}</p>
              <small>{{ closed_ticket.created_at }}</small>
            </div>  
          </div>
        </div>
      </div>

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
        data: {},
        open_tickets: [],
        closed_tickets: []
      }
    },
    created() {
      this.getTickets();
    },
    watch: {
    // call again the method if the route changes
    '$route': 'getTickets'
    },
    methods: {
      async getTickets() {
        fetch(`https://${process.env.STAGING_URL}/api/v1/tickets`)
        .then(res => res.json())
        .then(data => {
          // console.log(data)
          this.open_tickets = data.open_tickets;
          this.closed_tickets = data.closed_tickets;
        })
        .catch(err => console.log(err))
      }
    }
  }
</script>