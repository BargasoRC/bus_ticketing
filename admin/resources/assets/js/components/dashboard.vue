<template>
  <v-card class="mx-auto" max-width="100%">
    <v-card class="mx-auto" max-width="95%" outlined>
      <v-data-table 
        item-key="name"
        :headers="headers" 
        :items="desserts" 
        sort-by="calories" 
        class="elevation-1" 
        :loading="loader" 
        loading-text="Loading... Please wait"
        :search="search"
      >
        <template v-slot:top>
          <v-toolbar flat color="white">
            <v-toolbar-title>Daily Bookings</v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-text-field
                v-model="search"
                append-icon="fas fa-search"
                label="Search"
                single-line
                hide-details
            ></v-text-field>
          </v-toolbar>
        </template>
        <template v-slot:item.action="{ item }">
            <v-icon small @click="releaseTicket(item)">
                far fa-check-circle
            </v-icon>&nbsp;&nbsp;
          <v-icon small @click="holdTicket(item)">
              fas fa-thumbtack
          </v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary" @click="initialize">Reset</v-btn>
        </template>
      </v-data-table>
    </v-card>
    <v-card class="bookings mx-auto" max-width="95%" outlined>
      <apexchart width="100%" type="bar" :options="options" :series="series"></apexchart>
    </v-card>
  </v-card>
</template>
<style scoped>
.bookings {
  margin-top: 20px;
}
</style>

<script>
import Vue from "vue";
import VueApexCharts from "vue-apexcharts";
Vue.component("apexchart", VueApexCharts);
export default {
  data: () => ({
    options: {
      chart: {
        id: "vuechart-example"
      },
      xaxis: {
        categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998]
      }
    },
    series: [
      {
        name: "series-1",
        data: [30, 40, 45, 50, 49, 60, 70, 91]
      }
    ],
    dialog: false,
    loader:true,
    search: '',
    headers: [
      {
        text: "Passenger",
        align: "left",
        sortable: true,
        value: "name"
      },
      { text: "Ticket No.", value: "ticketno" },
      { text: "Bus Number", value: "busNumber" },
      { text: "Passenger Type", value: "passengerType" },
      { text: "From", value: "from" },
      { text: "To", value: "to" },
      { text: "Fare", value: "fare" },
      { text: "Status", value: "status" },
      { text: "Action", value: "action", sortable: false }
    ],
    desserts: [],
    defaultItem: {
      name: "",
      ticketno:0,
      busNumber: 0,
      passengerType: 0,
      from:0,
      to:0,
      fare: 0,
      status:""
    }
  }),

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
        if(this.desserts !== []){
            this.loader = false;
        }
        this.desserts = [
            {
                name: "Renan Bargaso",
                ticketno:'GTX-998-45',
                busNumber: '9956',
                passengerType: 'Regular',
                from:'Cebu',
                to:'Dumaguete',
                fare: 250,
                status:'pending'
            },
            {
                name: "Renan Cañete",
                ticketno:'FORD-99Y-3',
                busNumber: '9956',
                passengerType: 'Student',
                from:'Cebu',
                to:'Dumaguete',
                fare: 210,
                status:'pending'
            }
        ];
    },

    holdTicket(item) {
      const index = this.desserts.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.desserts.splice(index, 1);
    },

    releaseTicket(item) {
        const index = this.desserts.indexOf(item);
        this.desserts[index].status = 'released';
    }

  }
};
</script>