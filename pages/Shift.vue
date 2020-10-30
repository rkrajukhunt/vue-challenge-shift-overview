<template>
  <v-card>
    <v-toolbar dark color="primary">
      <v-btn icon dark @click="close">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title>{{
        !isEdit ? "Create Shift" : "Edit Shift"
      }}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn dark text @click="saveShift">Save</v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <v-container>
      <v-row>
        <v-row justify="center" align="center">
          <v-col cols="12">
            <v-text-field
              label="Title*"
              v-model="title"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-textarea
              label="Description*"
              v-model="description"
              required
            ></v-textarea>
          </v-col>
          <v-col cols="12" sm="6">
            <v-menu
              ref="menu"
              v-model="menu"
              :close-on-content-click="false"
              :return-value.sync="dates"
              transition="scale-transition"
              offset-y
              min-width="290px"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-combobox
                  v-model="dates"
                  multiple
                  chips
                  small-chips
                  label="Multiple picker in menu"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                ></v-combobox>
              </template>
              <v-date-picker v-model="dates" multiple no-title scrollable>
                <v-spacer></v-spacer>
                <v-btn text color="primary" @click="menu = false">
                  Cancel
                </v-btn>
                <v-btn text color="primary" @click="saveDates(dates)">
                  OK
                </v-btn>
              </v-date-picker>
            </v-menu>
          </v-col>

          <v-col>
            <div v-for="(date, index) in selectedDates" :key="index">
              <p>{{ date.date }}</p>
              <v-row>
                <v-col md="4">
                  <v-menu
                    ref="menu2"
                    v-model="menu2"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    :return-value.sync="date.time"
                    transition="scale-transition"
                    offset-y
                    max-width="290px"
                    min-width="290px"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="date.time"
                        label="Picker in menu"
                        prepend-icon="mdi-clock-time-four-outline"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    {{ time }}
                    <v-time-picker
                      v-model="time"
                      full-width
                      @click="saveTime(time, date)"
                    ></v-time-picker>
                  </v-menu>
                </v-col>
                <v-col md="4">
                  <v-menu
                    ref="menu2"
                    v-model="menu2"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    :return-value.sync="date.time"
                    transition="scale-transition"
                    offset-y
                    max-width="290px"
                    min-width="290px"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="date.time"
                        label="Picker in menu"
                        prepend-icon="mdi-clock-time-four-outline"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    {{ time }}
                    <v-time-picker
                      v-model="time"
                      full-width
                      @click="saveTime(time, date)"
                    ></v-time-picker>
                  </v-menu>
                </v-col>
                <v-col md="4">
                  <v-text-field
                    label="Price*"
                    v-model="price"
                    required
                  ></v-text-field>
                </v-col>
                <v-col md="6">
                  <v-select
                    :items="types"
                    v-model="type"
                    label="Type"
                  ></v-select>
                </v-col>
              </v-row>
            </div>
          </v-col>
        </v-row>
      </v-row>
    </v-container>
  </v-card>
</template>

<script>
const initialState = {
  title: "",
  description: "",
  price: "",
  type: "",
  dates: [
    {
      date: "",
      time: "",
      type: ""
    }
  ]
};
export default {
  data() {
    return {
      dates: [],
      menu: false,
      time: null,
      title: "",
      description: "",
      price: "",
      type: "",
      menu2: false,
      types: ["Consultation", "Telephone", "Ambulance"]
    };
  },
  props: {
    isEdit: {
      type: Boolean,
      default: false
    },
    data: {
      type: Object,
      default: initialState
    }
  },
  computed: {
    selectedDates() {
      let dates = [];
      if (this.dates.length > 0) {
        this.dates.map(obj => {
          dates.push({
            starttime: obj.starttime ? obj.starttime : "",
            endtime: obj.endtime ? obj.endtime : "",
            price: obj.price ? obj.price : "",
            type: "Consultation",
            date: obj
          });
        });
      }
      return dates;
    }
  },
  methods: {
    saveDates(dates) {
      this.menu = false;
      this.$refs.menu.save(dates);
    },
    saveTime(time, date) {
      console.log(this.$refs);
      this.$refs.menu2.save(time);
    },
    saveShift() {
      this.$store.dispatch("shift/setShifts", {
        title: this.title,
        description: this.description,
        price: this.price,
        type: this.type,
        id:new Date().getUTCMilliseconds()
      });
      this.close();
    },
    close() {
      this.$emit("closeDailog");
    }
  }
};
</script>
