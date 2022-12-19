<template>
  <div>
    <h3>Customers</h3>
    <input type="text" v-model="search" placeholder="Search for names..." />
    <table v-if="customers.length > 0">
      <tr>
        <th>name</th>
        <th>address</th>
      </tr>
      <tr v-for="customer in customers" :key="customer.id">
        <td>{{ customer.name }}</td>
        <td>{{ customer.address }}</td>
      </tr>
    </table>
    <div class="center">
      <p>Customer</p>
      <select v-if="customers.length > 0">
        <option v-for="customer in customers" :key="customer.id">
          {{ customer.name }}
        </option>
      </select>
    </div>
    <table v-if="invoice_customers.length > 0" class="table table-striped">
      
      <tr>
        <th>Id</th>
        <th>Amount</th>
        <th>Expire Date</th>
        <th>Note</th>
      </tr>
      
      
      <tr
        v-for="invoice_customer in invoice_customers"
        :key="invoice_customer.id"
      >
      
        <td>
            <input
              type="number"
              v-model="invoice_customers.amount"
            />
        
        </td>
        <td>
           <input type="date" v-model="invoice_customers.expire_date">
        </td>
        <td>
           <input type="text" v-model="invoice_customers.note"/>
        </td>
        <td>
          <i class="far fa-trash-alt" @click="deleteInstallment(invoice_customer)"> </i>
        </td>
      </tr>
    </table>

    <b-button variant="outline-success" @click="addNewInstallment"
      >+ Add New</b-button
    >
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      selected: "",
      customers: [],
      search: "",

      invoice_customers: [
        {
          id: 0,
          amount: 0,
          expire_date: "",
          note: "",
        },
      ],
    };
  },
  props: {},

  async mounted() {
    const response = await this.$axios.$get("/api/customers");
    this.customers = response.data;
    return this.customers.filter((p) => {
      // return true if the product should be visible

      // in this example we just check if the search string
      // is a substring of the product name (case insensitive)
      return p.name.toLowerCase().indexOf(this.search.toLowerCase()) != -1;
    });
  },

  methods: {
    addNewInstallment() {
      this.invoice_customers.push({
        id: 0,
        amount: 0,
        expire_date: "",
        note: "",
      });
    },

    deleteInstallment(index, invoice_customer) {
      var idx = this.invoice_customers.indexOf(invoice_customer);
      if (idx > -1) {
      this.invoice_customers.splice(idx, 1);
      }
    }
  },
};
</script>

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 40%;
  margin-left: auto;
  margin-right: auto;
}

.center {
  margin-left: 10.2cm;
}

input {
  width: 40%;
  margin-left: 10.2cm;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}

h3 {
  text-align: center;
}
</style>