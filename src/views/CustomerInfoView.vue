<template>
  <div>

    <button v-on:click="hideTableDiv" type="button" class="btn btn-danger">Peida</button>
    <button v-on:click="displayTableDiv" type="button" class="btn btn-success m-3">Näita</button>

    <br>
    <br>

    <div v-if="tableDivDisplay">

      <table class="table table-hover">
        <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Eesnimi</th>
          <th scope="col">Perekonnanimi</th>
          <th scope="col">Isikukood</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="customer in customers">
          <th scope="row">*</th>
          <td>{{customer.firstName}}</td>
          <td>{{customer.lastName}}</td>
          <td>{{customer.isikukood}}</td>
          <button type="button" class="btn btn-primary" v-on:click="navigateToAccountsInfo(customer.id)">Vajuta siia</button>
        </tr>
        </tbody>
      </table>

    </div>



  </div>


</template>

<script>
export default {
  name: "CustomerInfo",
  data: function () {
    return {

      customerId: null,
      customer: {},
      customers: {},
      tableDivDisplay: true
    }
  },
  methods: {

    displayTableDiv: function () {
      this.tableDivDisplay = true
    },
    hideTableDiv: function () {
      this.tableDivDisplay = false
    },

    getAllCustomers: function () {
      this.$http.get('/customer/all')
          .then(response => {
            this.customers = response.data
            console.log(response.data)
      })
          .catch(error => console.log(error))
    },

    navigateToAccountsInfo: function (customerId) {
      console.log('see on customer ID ' + customerId)
      this.$router.push({name: 'accountRoute', query:{id:customerId}})
    },


    findCustomerById: function () {
      this.$http.get('customer/id', {
        params: {
          id: this.customerId
        }
      })
          .then(response => {
        this.customer = response.data
        console.log(response.data)
      })
          .catch(error => console.log(error))
    }

  },
  mounted() {
    this.getAllCustomers()
  }
}

</script>

