<template>
  <div class="mb-5">
    <v-card class="p-5 mb-5" v-for="(profile, key1) in member" :key="key1">
      <v-container class="d-flex">
        <div style="width: 40%">
          <v-img :src="profile.Foto"></v-img>
        </div>
        <v-simple-table style="width: 100%">
          <tbody>
            <template v-for="(value, key) in profile">
              <tr :key="key" v-if="key != 'Foto'">
                <td>{{ key }}</td>
                <td>{{ value }}</td>
              </tr>
            </template>
          </tbody>
        </v-simple-table>
      </v-container>
    </v-card>
  </div>
</template>
<script>
export default {
  data() {
    return {
      member: [
        {
          Foto: "img/foto_eri.jpg",
          Nama: "Eriawan Hidayat",
          NIM: "D111921136",
          Kelas: "IF-4K",
          Agama: "Islam",
          Alamat: "Padalarang",
          Pekerjaan: "Back-End Developer",
        },
        {
          Foto: "img/foto_iqbal.jpeg",
          Nama: "Mochamad Iqbal",
          NIM: "D111921126",
          Kelas: "IF-4K",
          Agama: "Islam",
          Alamat: "Cihanjuang",
          Pekerjaan: "Front-End Developer",
        },
      ],
    };
  },
};
</script>
