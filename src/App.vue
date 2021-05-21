<template>
  <div id="main-app" class="container">
    <h4>{{ title }}</h4>
    <div class="row justify-content-center">
      <add-appointment @add="addItem" />
      <appointment-list
        :appointments="this.appointments"
        @remove="removeItem"
        @edit="editItem"
      />
    </div>
  </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import AppointmentList from "./components/AppointmentList";
import axios from "axios";
import _ from "lodash";
import AddAppointment from "./components/AddAppointment.vue";

export default {
  name: "MainApp",
  data: function () {
    return {
      title: "Appointment List",
      appointments: [],
      aptId: 0,
      components: {
        FontAwesomeIcon,
      },
    };
  },
  components: {
    AppointmentList,
    AddAppointment,
  },
  mounted() {
    axios.get("./data/appointments.json").then(
      (res) =>
        (this.appointments = res.data.map((item) => {
          this.aptId++;
          return {
            aptId: this.aptId,
            ...item,
          };
        }))
    );
  },
  methods: {
    addItem: function (apt) {},
    removeItem: function (apt) {
      this.appointments = _.without(this.appointments, apt);
    },
    editItem: function (id, field, text) {
      const aptIndex = _.findIndex(this.appointments, {
        aptId: id,
      });

      this.appointments[aptIndex][field] = text;
    },
  },
};
</script>

<style>
</style>
