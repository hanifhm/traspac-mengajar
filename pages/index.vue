<template>
  <div>
    <v-card flat class="rounded-xl">
      <v-card-title class="d-flex align-center justify-space-between">
        <span class="text-h6 grey--text">Daftar Siswa</span>
        <v-btn fab small color="success" depressed>
          <v-icon color="white" size="30">
            mdi-plus
          </v-icon>
        </v-btn>
      </v-card-title>
      <v-card-text>
        <v-data-table
          height="70vh"
          :headers="headers"
          :items="items"
          fixed-header
        >
          <template #[`item.no`]="{index}">
            <div class="text-center">
              {{ index + 1 }}
            </div>
          </template>
          <template #[`item.action`]>
            <div class="d-flex align-center">
              <v-btn icon>
                <v-icon>
                  mdi-pencil
                </v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>
                  mdi-delete
                </v-icon>
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
  name: 'IndexPage',
  data: () => ({
    loading: false,
    openDialog: false,
    items: [],
    headers: [
      {
        text: 'No',
        value: 'no',
        sortable: false,
        width: 50,
        align: 'center'
      },
      {
        text: 'NIS',
        value: 'nis',
        sortable: false,
        width: 100,
        align: 'center'
      },
      {
        text: 'Nama',
        value: 'nama',
        sortable: false,
        width: 700,
        align: 'center'
      },
      {
        text: 'Kelas',
        value: 'kelas',
        sortable: false,
        width: 100,
        align: 'center'
      },
      {
        text: '',
        value: 'action',
        sortable: false
      }
    ]
  }),
  created () {
    //
  },
  methods: {
    //
  }
}
</script>
