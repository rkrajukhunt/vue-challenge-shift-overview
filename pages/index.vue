<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="6">
      <div>
        <h3>Shifts</h3>
        <v-spacer></v-spacer>
        <v-btn icon class="float-right" @click="openModal('CREATE', {})">
          <v-icon fab>mdi-plus</v-icon>
        </v-btn>
        <v-spacer></v-spacer>
        <v-card
          class="mx-auto mt-10"
          width="900"
          outlined
          v-for="(data, index) in shiftList"
          :key="index"
        >
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1">
                {{ data.title }}
              </v-list-item-title>
              <v-list-item-subtitle>{{
                data.description
              }}</v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-avatar tile size="30">
              <v-btn @click="openModal('EDIT', data)">
                <v-icon>mdi-pencil</v-icon>
              </v-btn></v-list-item-avatar
            >
          </v-list-item>
          <!-- <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1">
                Dates
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item> -->
          <!-- <v-simple-table dark>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-center"></th>
                  <th class="text-center"></th>
                  <th class="text-center"></th>
                  <th class="text-center"></th>
                  <th class="text-center"></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in [1, 2, 3, 4]" :key="item">
                  <td>{{ item }}</td>
                  <td>{{ item }}</td>
                  <td>{{ item }}</td>
                  <td>{{ item }}</td>
                  <td>{{ item }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table> -->
        </v-card>
      </div>
    </v-col>

    <!-- Shift add/edit dialog start -->
    <v-dialog
      v-model="isOpenDialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <Shift
        :isEdit="UIconcern === 'CREATE' ? false : true"
        :data="UIconcern === 'CREATE' ? {} : selectedShiftData"
        @closeDailog="closeDialog"
      />
    </v-dialog>
    <!-- Shift add/edit dialog end -->
  </v-row>
</template>

<script>
import { mapGetters } from "vuex";
// components
import Shift from "./Shift.vue";
export default {
  data() {
    return {
      isOpenDialog: false,
      UIconcern: "CREATE",
      selectedShiftData: {}
    };
  },
  components: {
    Shift
  },
  computed: {
    ...mapGetters({
      shiftList: "shift/getShiftList"
    })
  },
  methods: {
    openModal(UIconcern, data) {
      this.isOpenDialog = true;
      this.UIconcern = UIconcern;
      if (this.UIconcern === "EDIT") {
        this.selectedShiftData = data;
      } else {
        this.selectedShiftData = {};
      }
    },
    closeDialog() {
      this.isOpenDialog = false;
    }
  }
};
</script>
