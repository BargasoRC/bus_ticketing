<template>
  <v-card class="mx-auto" max-width="600">
    <v-card-title>Bus Registration</v-card-title>
    <v-card-text class="text--primary" width="70%">
      <form>
        <v-text-field
          v-model="name"
          :error-messages="nameErrors"
          label="Bus Name"
          prepend-inner-icon="fas fa-bus-alt"
          required
          @input="$v.name.$touch()"
          @blur="$v.name.$touch()"
        ></v-text-field>
        <v-text-field
          v-model="busNo"
          :error-messages="busNoErrors"
          label="Bus Number"
          prepend-inner-icon="fas fa-bus-alt"
          required
          @input="$v.busNo.$touch()"
          @blur="$v.busNo.$touch()"
        ></v-text-field>
        <v-row>
          <v-col cols="12">
            <v-dialog
              ref="dialog1"
              v-model="start_time_modal"
              :return-value.sync="start_time"
              persistent
              width="290px"
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  v-model="start_time"
                  label="Departure Time"
                  :error-messages="departureErrors"
                  prepend-inner-icon="fas fa-clock"
                  @input="$v.start_time.$touch()"
                  @blur="$v.start_time.$touch()"
                  readonly
                  required
                  v-on="on"
                ></v-text-field>
              </template>
              <v-time-picker v-if="start_time_modal" v-model="start_time" full-width>
                <v-spacer></v-spacer>
                <v-btn text color="primary" @click="start_time_modal = false">Cancel</v-btn>
                <v-btn text color="primary" @click="$refs.dialog1.save(start_time)">OK</v-btn>
              </v-time-picker>
            </v-dialog>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-dialog
              ref="dialog2"
              v-model="arrival_time_modal"
              :return-value.sync="arrival_time"
              persistent
              width="290px"
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  v-model="arrival_time"
                  label="Arrival Time"
                  :error-messages="arrivalErrors"
                  prepend-inner-icon="fas fa-clock"
                  @input="$v.arrival_time.$touch()"
                  @blur="$v.arrival_time.$touch()"
                  readonly
                  required
                  v-on="on"
                ></v-text-field>
              </template>
              <v-time-picker v-if="arrival_time_modal" v-model="arrival_time" full-width>
                <v-spacer></v-spacer>
                <v-btn text color="primary" @click="arrival_time_modal = false">Cancel</v-btn>
                <v-btn text color="primary" @click="$refs.dialog2.save(arrival_time)">OK</v-btn>
              </v-time-picker>
            </v-dialog>
          </v-col>
        </v-row>
        <v-text-field
          v-model="from"
          :error-messages="fromErrors"
          label="Journey From"
          prepend-inner-icon="fas fa-map-marker-alt"
          required
          @input="$v.from.$touch()"
          @blur="$v.from.$touch()"
        ></v-text-field>
        <v-text-field
          v-model="to"
          :error-messages="toErrors"
          label="Journey To"
          prepend-inner-icon="fas fa-map-marked-alt"
          required
          @input="$v.to.$touch()"
          @blur="$v.to.$touch()"
        ></v-text-field>
        <v-text-field
          v-model="sp"
          :error-messages="spErrors"
          label="Non-regular Fare"
          prepend-inner-icon="far fa-money-bill-alt"
          required
          @input="$v.sp.$touch()"
          @blur="$v.sp.$touch()"
        ></v-text-field>
        <v-text-field
          v-model="regular"
          :error-messages="regularErrors"
          label="Regular Fare"
          prepend-inner-icon="far fa-money-bill-alt"
          required
          @input="$v.regular.$touch()"
          @blur="$v.regular.$touch()"
        ></v-text-field>
        <v-text-field
          v-model="seats"
          :error-messages="seatsErrors"
          label="Number of Seats"
          prepend-inner-icon="fas fa-chair"
          required
          @input="$v.seats.$touch()"
          @blur="$v.seats.$touch()"
        ></v-text-field>
        <v-btn class="mr-4" @click="submit">submit</v-btn>
        <v-btn @click="clear">clear</v-btn>
      </form>
    </v-card-text>
  </v-card>
</template>
<script>
import { validationMixin } from "vuelidate";
import { required, maxLength, integer } from "vuelidate/lib/validators";

export default {
  mixins: [validationMixin],

  validations: {
    name: { required },
    busNo: { required },
    select: { required },
    start_time: { required },
    arrival_time: { required },
    from: { required },
    to: { required },
    sp:{ required, integer },
    regular: { required, integer },
    seats: { required, integer }
  },

  data: () => ({
    name: "",
    busNo: "",
    start_time: "",
    start_time: null,
    start_time_modal: false,
    arrival_time: null,
    arrival_time_modal: false,
    from: "",
    to: "",
    sp: "",
    regular: "",
    seats:""
  }),

  computed: {
    spErrors() {
      const errors = [];
      if (!this.$v.sp.$dirty) return errors;
      !this.$v.sp.required && errors.push("Fare is required");
      !this.$v.sp.integer && errors.push("Fare should be a number")
      return errors;
    },
    regularErrors() {
      const errors = [];
      if (!this.$v.regular.$dirty) return errors;
      !this.$v.regular.required && errors.push("Fare is required");
      !this.$v.regular.integer && errors.push("Fare should be a number")
      return errors;
    },
    nameErrors() {
      const errors = [];
      if (!this.$v.name.$dirty) return errors;
      !this.$v.name.required && errors.push("Bus Name is required.");
      return errors;
    },
    busNoErrors() {
      const errors = [];
      if (!this.$v.busNo.$dirty) return errors;
      !this.$v.busNo.required && errors.push("Bus Number is required.");
      return errors;
    },
    fromErrors() {
      const errors = [];
      if (!this.$v.from.$dirty) return errors;
      !this.$v.from.required && errors.push("Journey From is required.");
      return errors;
    },
    departureErrors(){
        const errors = [];
        if (!this.$v.start_time.$dirty) return errors;
        !this.$v.start_time.required && errors.push("Departure Time is required.");
        return errors;
    },
    arrivalErrors(){
        const errors = [];
        if (!this.$v.arrival_time.$dirty) return errors;
        !this.$v.arrival_time.required && errors.push("Arrival Time is required.");
        return errors;
    },
    toErrors() {
      const errors = [];
      if (!this.$v.to.$dirty) return errors;
      !this.$v.to.required && errors.push("Journey To is required.");
      return errors;
    },
    seatsErrors(){
        const errors = [];
        if (!this.$v.seats.$dirty) return errors;
        !this.$v.seats.required && errors.push("Number of seats is required.");
        !this.$v.seats.integer && errors.push("Seats should be number");
        return errors;
    }
  },

  methods: {
    submit() {
      this.$v.$touch();
    },
    clear() {
        this.$v.$reset();
        this.name = "";
        this.email = "";
        this.start_time =  "";
        this.start_time = null;
        this.arrival_time = null;
        this.from = "";
        this.to = "";
        this.sp = "";
        this.regular = "";
        this.seats = ""
    }
  }
};
</script>