<template>
  <div class="en-merchantForm">
    <div class="en-merchantForm-title">
      <v-card elevation="0" class="pa-5 mb-5 border">
        <v-form ref="form" v-model="valid">
            <v-row>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-form-textbox"
                  v-model="Name"
                  :success="!!Name"
                  :rules="[
                    (v) => !!v || 'Merchant Name is required',
                  ]"
                  label="Merchant Name"
                  required
                />
              </v-col>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-translate"
                  v-model="arName"
                  :success="!!arName"
                  :rules="[
                    (v) => !!v || 'Arabic Merchant Name is required',
                  ]"
                  label="Arabic Merchant Name"
                  required
                />                    
              </v-col>
            </v-row>
            <v-divider></v-divider>
            <v-row>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-cellphone"
                  v-model="Phone"
                  :success="!!Phone"
                  :rules="[
                    (v) => !!v || 'Phone Number is required',
                    (v) =>
                      (v && /^[0-9]+$/.test(v)) ||
                      'Phone Number must be digits only',
                  ]"
                  label="Phone Number"
                  required
                />
              </v-col>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-email"
                  v-model="Email"
                  :success="!!Email"
                  label="Email"
                  :rules="[
                    (v) => !!v || 'E-mail is required',
                    (v) => /.+@.+\..+/.test(v) || 'E-mail must be valid',
                  ]"
                  required
                />                    
              </v-col>
            </v-row>
            <v-divider></v-divider>
            <v-row>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-map-marker"
                  v-model="Address"
                  :success="!!Address"
                  label="Address"
                  :rules="[(v) => !!v || 'Address is required']"
                  required
                />

              </v-col>
              <v-col>
                  <v-text-field
                    outlined dense
                    prepend-icon="mdi-translate"
                    v-model="arAddress"
                    :success="!!arAddress"
                    label="Address in Arabic"
                    :rules="[(v) => !!v || 'arAddress is required']"
                    required
                  />
                
              </v-col>
            </v-row>
            <v-divider></v-divider>
            <v-row>
              <v-col>
                <v-select
                  outlined dense
                  prepend-icon="mdi-format-list-bulleted-type"
                  v-model="Category"
                  :success="!!Category"
                  label="Category"
                  :rules="[(v) => !!v || 'Category is required']"
                  required
                  :items="flatCategories"
                  item-value="_id"
                  item-text="name"
                />
              </v-col>
              <v-col>
                <header class="h5">Account Type</header>
                <v-radio-group v-model="AccountType" row>
                  <v-radio
                    v-for="accType in accTypes"
                    :key="accType.val"
                    :label="accType.name"
                    :value="accType.val"
                  ></v-radio>
                </v-radio-group>
              </v-col>
            </v-row>
            <v-divider></v-divider>
            <v-row>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-web"
                  v-model="website"
                  :success="!!website"
                  label="Website"
                />

              </v-col>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-facebook"
                  v-model="socialMediaLinks.facebook"
                  :success="!!socialMediaLinks.facebook"
                  label="Facebook"
                />
                
              </v-col>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-youtube"
                  v-model="socialMediaLinks.youtube"
                  :success="!!socialMediaLinks.youtube"
                  label="Youtube"
                />

              </v-col>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-instagram"
                  v-model="socialMediaLinks.instagram"
                  :success="!!socialMediaLinks.instagram"
                  label="Instagram"
                />
                
              </v-col>
            </v-row>
            <v-divider></v-divider>
            <v-row>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-account"
                  v-model="username"
                  :rules="[(v) => !!v || 'Username is required']"
                  label="Username"
                  required
                />

              </v-col>
              <v-col>
                <v-text-field
                  outlined dense
                  prepend-icon="mdi-lock"
                  v-model="password"
                  :rules="[(v) => !!v || 'Password is required']"
                  label="Password"
                  required
                  type="password"
                /> 
              </v-col>
            </v-row>
            <v-divider></v-divider>




                <div class="en-singleBranch-main-right" >
                  <!-- <v-text-field
                    id="search-field-map-input"
                    v-model="place"
                    :success="placeIsCorrect"
                    :rules="[(v) => !!v || 'Place is required']"
                    label="Search for a place"
                    placeholder="Search for a place"
                  /> -->
                  <map-search-place @onSelectedNewPlace="handleSelectedNewPlace"/>
                  
                </div>

                <form
                  class="en-merchantForm-main-left-form-row"
                  enctype="multipart/form-data">
                  <v-file-input
                    outlined
                    dense
                    ref="file"
                    v-model="file"
                    :success="!!file"
                    accept="image/png, image/jpeg, image/bmp"
                    placeholder="Pick a logo"
                    prepend-icon="mdi-camera"
                    label="Pick a Logo"
                    @change="onFileChange"
                  />
                </form>
                <v-divider class='my-8'></v-divider>
                <v-card class="p-3">
                  <v-card-actions>
                    <v-btn color="primary" small @click="addPayment()">Add Payment</v-btn>
                  </v-card-actions>
                  <v-card-text>
                    <header class="h5">Payment</header>
                    <div v-for="(payment,index) in payments" :key="'p'+index">
                    <v-row justify="space-around">
                      <v-row>
                        <v-col>
                          <v-text-field outlined dense
                            prepend-icon="mdi-cash" 
                            v-model="payment.amount"  
                            label="Amount"/>
                        </v-col>
                        <v-col>
                          <v-text-field outlined dense
                            prepend-icon="mdi-calender" 
                            v-model="payment.dateGet"  
                            label="Date" 
                            type="datetime-local"/>                          
                        </v-col>
                      </v-row>
                    </v-row>
                    <v-row justify="space-around">
                      <v-text-field outlined dense
                          prepend-icon="mdi-account-convert" 
                          v-model="payment.description"  
                          label="By who"/>
                    </v-row>
                      <v-btn color="error" small @click="$delete(payments, index)">remove</v-btn>
                      <hr>
                    </div>
                  </v-card-text>
                </v-card>
        </v-form>
      </v-card>
    </div>
  </div>
</template>

<script>
import { required, integer, email } from "vuelidate/lib/validators"
import GoogleMapsNative from "~/components/en/General/GoogleMapsNative.vue"
import notification from "~/components/en/General/notification.vue"
import MapSearchPlace from "@/components/map-search-place.vue"

export default {
  name: "MerchantForm",
  components: {
    GoogleMapsNative,
    notification,
    MapSearchPlace,
  },
  props: {
    showMap: {
      type: Boolean,
      default: false,
    },
    validateMerchant: {
      type: Boolean
    }
  },
  data () {
    return {
      lat: 30.04,
      long: 31.23,
      isMapLoaded1: false,
      isMapLoaded2: false,
      isMapLoaded3: false,
      isMapLoaded4: false,
      categories: "",
      location_status: "not selected",
      location: {},
      socialMediaLinks: {
        facebook: "",
        youtube: "",
        instagram: "",
      },
      flatCategories: [],
      Name: "",
      arName:"",
      Phone: "",
      website: "",
      Email: "",
      Address: "",
      arAddress:"",
      Category: "some category",
      username: "",
      password: "",
      image: "",
      AccountType: "",
      success: "",
      error: "",
      suggestions: "",
      place: "",
      selected_place: "",
      file: [],
      accTypes: [
        { name: "Premium", val: "Premium", },
        { name: "Free", val: "Free", }
      ],
      placeIsCorrect: false,
      fileAdded: false,
      valid: false,
      payments: [{
        dateGet: '',
        amount: '',
        description: '',
      }]
    }
  },
  watch: {
    validateMerchant () {
      this.validateForm()
    },
  },
  mounted () {
    // this.initAutocompleteMapInput();
    this.$axios.get("/categories", {
      headers: { authorization: this.$store.getters.token, },
    }).then((data) => {
      this.categories = data.data.data
      if (this.categories.length) {
        this.flattenCategoriesRecursively(this.categories)
      } else {
      }
    })
  },
  methods: {
    addPayment() {
      this.payments.push({
        dateGet: '',
        amount: '',
        description: '',
      })
    },
    validateForm() {
      if (this.$refs.form.validate() && this.fileAdded) {
        this.$emit("merchantValidated", true)
      } else {
        this.$emit("merchantValidated", false)
      }
    },
    getAddressData (addressData, placeResultData, id) {
      this.lat = addressData.latitude
      this.long = addressData.longitude
      this.location = {
        lat: this.lat.toString(),
        long: this.long.toString(),
      }
      this.placeIsCorrect = true
      // this.confirmBranch()
    },
    SearchForAplace () {
      this.suggestions = ""
      const getStatment =
        "https://places.ls.hereapi.com/places/v1/autosuggest?apiKey=d9JJdfnwO3m7HTjkm2tkS7WYy0Rkvu2hQQ2Ku643QZw&at=40.7539,-73.9837&q=" +
        this.place +
        "&pretty"
      this.$axios
        .get(getStatment)
        .then((result) => {
          this.suggestions = result.data.results
          try {
            this.suggestions.map((suggest) => {
              suggest.vicinity = suggest.vicinity.replace(/<\/?[^>]+(>|$)/g, "")
              suggest.id = Math.random() * 1000
            })
          } catch(err) {
            this.$store.dispatch('settings/activeMsg', { text: err.message, color: 'red'})
          }
        })
        .catch((err) => {
          this.$store.dispatch('settings/activeMsg', { text: err.message, color: 'red'})
        })
    },
    clickForPlace (e) {
      try {
        this.suggestions = ""
        const myarray = e.target.getAttribute("posTitle").split(",")
        this.place = e.target.getAttribute("showTitle")
        this.selected_place = e.target.getAttribute("showtitle")
        const position = {
          lat: myarray[0],
          long: myarray[1],
        }
        this.lat = parseInt(position.lat)
        this.long = parseInt(position.long)
        this.location = {
          lat: position.lat.toString(),
          long: position.long.toString(),
        }
        if (this.location) {
          this.location_status = 'selected';
        }
      } catch(err) {
        this.$store.dispatch('settings/activeMsg', { text: err.message, color: 'red'})
      }
    },

    createImage (file) {
      // eslint-disable-next-line no-unused-vars
      const image = new Image()
      const reader = new FileReader()
      const vm = this
      reader.onload = (e) => {
        vm.image = this.file
      }
      reader.readAsDataURL(file)
    },
    removeImage (e) {
      this.image = ""
    },
    onFileChange () {
      if (this.file) {
        this.createImage(this.file)
        // this.$store.commit("updateCover", this.file)
        // this.$store.state.merchantData.cover = this.file
        this.fileAdded = true
      } else {
        this.fileAdded = false
      }
    },
    flattenCategoriesRecursively (items) {
      items.forEach((cat) => {
        this.flatCategories.push(cat)
        if (cat.root.length) { this.flattenCategoriesRecursively(cat.root) } else {}
      })
    },
    initAutocompleteMapInput() {
      // Create the search box and link it to the UI element.
      const searchInput = document.querySelector("#search-field-map-input");
      if (!searchInput) {
        return;
      }
      this.mapInputAutocomplete = new google.maps.places.SearchBox(searchInput);

      // Listen for the event fired when the user selects a prediction and retrieve
      // more details for that place.
      this.mapInputAutocomplete.addListener("places_changed", () => {
        const place = this.mapInputAutocomplete.getPlaces()[0];
        if ((place.geometry || {}).location) {
          this.place = place.name
          this.location = JSON.parse(JSON.stringify(place.geometry.location));
          this.location_status = "selected"
        } else {
          searchInput.value = "";
        }
      });
    },
    handleSelectedNewPlace(data) {
      this.location_status = "selected";
      this.location = data.location;
      this.place = data.address
    }
  },
  // validations: {
  //   Name: {
  //     required,
  //   },
  //   Phone: {
  //     required,
  //     integer,
  //   },
  //   Email: {
  //     required,
  //     email,
  //   },
  //   Address: {
  //     required,
  //   },
  //   Category: {
  //     required,
  //   },
  //   username: {
  //     required,
  //   },
  //   password: {
  //     required,
  //   },
  //   AccountType: {
  //     required,
  //   },
  // },
}
</script>

<style lang="scss" scoped>
.maps-suggestions {
  width: 85%;
  background-color: rgba(204, 204, 204, 0);
  padding: 5px;
  color: rgb(45, 105, 233);
  border-radius: 5px;
  position: float;
  height: 200px;
  overflow-y: scroll;
}

.map-suggest {
  text-align: center;
  font-size: 20px;
  cursor: pointer;
}
.en-merchantForm {
  &-msg {
    margin-bottom: 10px;
  }
  &-title {
    font-size: 24px;
  }
  &-main {
    width: 100%;
    display: flex;
    flex-flow: row;
    justify-content: space-between;
    &-left {
      // background-color: red;
      width: 48%;
      &-form {
        display: flex;
        flex-flow: column;
        justify-content: space-around;
        &-row {
          margin-bottom: 0px;
        }
      }
    }
    &-right {
      width: 48%;
    }
  }
  .field-error {
    font-size: 15px;
    color: red;
  }
}
input[type="file"] {
  display: none;
}
.custom-file-upload {
  display: inline-block;
  padding: 8px 30px;
  text-align: center;
  background-color: #dc1f29;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  font-size: 18px;
}
.custom-file-upload:hover {
  background-color: #ae2930;
}
.optional-title {
  color: #ccc;
  font-size: 15px;
}
.cover-img {
  width: 200px;
  width: 200px;
  margin: auto;
  display: block;
  margin-bottom: 10px;
}
.btn-font {
  font-size: 18px;
}
</style>
