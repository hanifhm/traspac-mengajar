<template>
  <div>
    <v-dialog
      v-model="openDialog"
      width="600px"
      class="rounded-lg"
      persistent
      eager
    >
      <v-card flat class="rounded-lg">
        <div class="grey--text pa-5 d-flex align-center justify-space-between">
          <span class="font-weight-regular grey--text">Tambah Siswa</span>
          <v-btn
            icon
            fab
            depressed
            small
            color="grey"
            @click="openDialog = false"
          >
            <v-icon size="30"> mdi-close </v-icon>
          </v-btn>
        </div>
        <div class="pa-3">
          <v-form ref="formSiswa">
            <v-row class="mt-n5">
              <v-col cols="3">
                <v-subheader>NIS</v-subheader>
              </v-col>
              <v-col cols="9">
                <v-card flat color="transparent">
                  <v-text-field v-model="formSiswa.nis" flat outlined dense />
                </v-card>
              </v-col>
            </v-row>
            <v-row class="mt-n5">
              <v-col cols="3">
                <v-subheader>Nama</v-subheader>
              </v-col>
              <v-col cols="9">
                <v-card flat color="transparent">
                  <v-text-field v-model="formSiswa.nama" flat outlined dense />
                </v-card>
              </v-col>
            </v-row>
            <v-row class="mt-n5">
              <v-col cols="3">
                <v-subheader>Kelas</v-subheader>
              </v-col>
              <v-col cols="9">
                <v-card flat color="transparent">
                  <v-text-field v-model="formSiswa.kelas" flat outlined dense />
                </v-card>
              </v-col>
            </v-row>
          </v-form>
        </div>
        <div class="d-flex align-center justify-center pb-3">
          <v-btn
            text
            depressed
            class="rounded-lg"
            color="grey"
            @click="resetFormSiswa"
          >
            Batal
          </v-btn>
          <v-btn
            depressed
            class="rounded-lg white--text"
            color="red"
            :loading="loading.simpan"
            @click="simpanSiswa"
          >
            Simpan
          </v-btn>
        </div>
      </v-card>
    </v-dialog>
    <v-card flat class="rounded-xl">
      <v-card-title class="d-flex align-center justify-space-between">
        <span class="text-h6 grey--text">Daftar Siswa</span>
        <v-btn fab small color="success" depressed @click="openDialog = true">
          <v-icon color="white" size="30"> mdi-plus </v-icon>
        </v-btn>
      </v-card-title>
      <v-card-text>
        <v-data-table
          height="70vh"
          :headers="headers"
          :items="items"
          fixed-header
        >
          <template #[`item.no`]="{ index }">
            <div class="text-center">
              {{ index + 1 }}
            </div>
          </template>
          <template #[`item.action`]="{ item }">
            <div class="d-flex align-center">
              <v-btn icon>
                <v-icon> mdi-pencil </v-icon>
              </v-btn>
              <v-btn icon @click="hapusSiswa(item.siswa_id)">
                <v-icon> mdi-delete </v-icon>
              </v-btn>
            </div>
          </template>
        </v-data-table>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data: () => ({
    loading: false,
    loadingSimpan: false,
    openDialog: false,
    formSiswa: {
      siswa_id: null,
      nis: null,
      nama: null,
      kelas: null,
    },
    items: [],
    headers: [
      {
        text: "No",
        value: "no",
        sortable: false,
        width: 50,
        align: "center",
      },
      {
        text: "NIS",
        value: "nis",
        sortable: false,
        width: 100,
        align: "center",
      },
      {
        text: "Nama",
        value: "nama",
        sortable: false,
        width: 700,
        align: "center",
      },
      {
        text: "Kelas",
        value: "kelas",
        sortable: false,
        width: 100,
        align: "center",
      },
      {
        text: "",
        value: "action",
        sortable: false,
      },
    ],
  }),
  computed: {
    payloadFormSiswa() {
      const form = {
        siswa_id: this.formSiswa.siswa_id,
        nis: this.formSiswa.nis,
        nama: this.formSiswa.nama,
        kelas: this.formSiswa.kelas,
      };

      return form;
    },
  },
  async created() {
    await this.loadSiswa();
  },
  methods: {
    async loadSiswa() {
      this.loading = true;

      const res = await this.$axios.$get(
        "https://nyoba-belajar-nuxt.herokuapp.com/siswa"
      );

      this.items = res.data;

      this.loading = false;
    },
    async simpanSiswa() {
      this.loadingSimpan = true;

      const res = await this.$axios.$post(
        "https://nyoba-belajar-nuxt.herokuapp.com/siswa/simpan",
        this.payloadFormSiswa
      );

      if (res.status) {
        this.loadSiswa();
        this.resetFormSiswa();
      }

      this.loadingSimpan = false;
    },
    async deleteSiswa(id) {
      this.loadingSimpan = true;

      const res = await this.$axios.$post(
        "https://nyoba-belajar-nuxt.herokuapp.com/siswa/hapus",
        { siswa_id: id }
      );

      if (res.status) {
        this.loadSiswa();
      }

      this.loadingSimpan = false;
    },
    resetFormSiswa() {
      this.$refs.formSiswa.reset();
      this.openDialog = false;
    },
  },
};
</script>
