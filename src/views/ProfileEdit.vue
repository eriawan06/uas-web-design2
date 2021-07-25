<template>
  <div>
    <v-subheader>Edit Profile</v-subheader>
    <v-container fluid>
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-text-field
          label="Name"
          v-model="name"
          :rules="nameRules"
          required
          append-icon="person"
        ></v-text-field>
        <v-textarea
          label="Address"
          v-model="address"
          :rules="addressRules"
          required
          auto-grow
          rows="3"
        ></v-textarea>
        <v-text-field
          label="Phone"
          v-model="phone"
          :rules="phoneRules"
          required
          append-icon="phone"
        ></v-text-field>
        <v-select
          v-model="province_id"
          :items="provinces"
          item-text="province"
          item-value="id"
          label="Province"
          persistent-hint
          single-line
        ></v-select>
        <v-select
          v-model="city_id"
          v-if="province_id > 0"
          :items="citiesByProvince"
          item-text="city"
          item-value="id"
          label="City"
          persistent-hint
          single-line
        ></v-select>
        <div class="text-xs-center">
          <v-btn color="primary" :disabled="!valid" @click="submit">
            Submit
            <v-icon right dark>person_add</v-icon>
          </v-btn>
          <v-btn @click="clear">Clear</v-btn>
        </div>
      </v-form>
    </v-container>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
export default {
  name: "profile-edit",
  data() {
    return {
      valid: true,
      name: "",
      nameRules: [
        (v) => !!v || "Name is required",
        (v) =>
          (v && v.length <= 255) || "Name must be less than 255 characters",
      ],
      address: "",
      addressRules: [
        (v) => !!v || "Address is required",
        (v) =>
          (v && v.length <= 255) || "Address must be less than 255 characters",
      ],
      phone: "",
      phoneRules: [
        (v) => !!v || "Phone is required",
        (v) =>
          (v && v.length <= 12) || "Address must be less than 12 characters",
      ],
      province_id: 0,
      city_id: 0,
    };
  },
  computed: {
    ...mapGetters({
      user: "auth/user",
      provinces: "region/provinces",
      cities: "region/cities",
    }),
    citiesByProvince() {
      let province_id = this.province_id;
      return this.cities.filter(function (city) {
        if (city.province_id == province_id) return city;
      });
    },
  },
  methods: {
    ...mapActions({
      setAlert: "alert/set",
      setStatusDialog: "dialog/setStatus",
      setAuth: "auth/set",
      setProvinces: "region/setProvinces",
      setCities: "region/setCities",
    }),
    close() {
      this.setStatusDialog(false);
    },
    submit() {
      if (this.$refs.form.validate()) {
        let user_id = this.user.id;
        let config = {
            headers: {
                'Authorization': 'Bearer '+this.user.api_token,
            },
        }
        let formData = new FormData();
        formData.set("name", this.name);
        formData.set("phone", this.phone);
        formData.set("address", this.address);
        formData.set("city_id", this.city_id);
        formData.set("province_id", this.province_id);

        this.axios
          .post("/user/edit?user_id="+user_id, formData, config)
          .then((response) => {
            let dataUser = response.data.data;
            this.setAuth(dataUser);
            this.setAlert({
              status: true,
              text: "register success",
              type: "success",
            });
            this.$router.push({ path: "/profile" });
          })
          .catch((err) => {
            console.log(err);
            let responses = err.response;
            this.setAlert({
              status: true,
              text: responses.data.message,
              type: "error",
            });
          });
      }
    },
    clear() {
      this.$refs.form.reset();
    },
  },
  created() {
    this.name = this.user.name;
    this.address = this.user.address;
    this.phone = this.user.phone;
    this.city_id = parseInt(this.user.city_id);
    this.province_id = parseInt(this.user.province_id);

    if (this.provinces && this.provinces.length == 0) {
      this.axios
        .get("/provinces")
        .then((response) => {
          this.setProvinces(response.data.data);
        })
        .catch((err) => {
          console.log(err);
        });

      this.axios
        .get("/cities")
        .then((response) => {
          this.setCities(response.data.data);
        })
        .catch((err) => {
          console.log(err);
        });
    }
  },
};
</script>