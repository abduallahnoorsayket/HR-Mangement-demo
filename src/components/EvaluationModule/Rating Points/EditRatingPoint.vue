<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-edit icon-gradient bg-tempting-azure"></i>
              </div>
              <div>
                 {{ $store.getters.GET_UPDATE +' '+$store.getters.GET_RATING+ ' '+$store.getters.GET_POINT}}
              </div>
            </div>
          </div>
        </div>
        <div class="main-card card">
          <div class="card-body">
            <form @submit.prevent="EditRatingPoint">
              <div class="form-group">
                <div class="form-row">
                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class>
                          {{$store.getters.GET_RATING+ ' '+$store.getters.GET_POINT+' '+ $store.getters.GET_NAME}}
                        <span class="asterisk">*</span>
                      </label>
                      <input
                        v-model="name"
                        type="text"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            department_error_data && department_error_data.name,}"
                      />
                      <!--error handling-->
                      <div
                        :class="{
                          'invalid-feedback':
                            department_error_data && department_error_data.name,
                        }"
                        v-if="
                          department_error_data && department_error_data.name"
                      >{{ department_error_data.name[0] }}</div>
                    </div>
                  </div>

                  <div class="col-md-6 position-relative form-group">
                    <label for="exampleEmail11" class>
                       {{$store.getters.GET_RATING+ ' '+$store.getters.GET_POINT+' '+ $store.getters.GET_VALUE}}
                      <span class="asterisk">*</span>
                    </label>
                    <input
                      v-model="value"
                      type="text"
                      class="form-control"
                      :class="{
                        'is-invalid':
                          department_error_data && department_error_data.value,
                      }"
                    />
                    <div
                      :class="{
                        'invalid-feedback':
                          department_error_data && department_error_data.value,
                      }"
                      v-if="
                        department_error_data && department_error_data.value
                      "
                    >{{ department_error_data.value[0] }}</div>
                  </div>
                </div>
                <div class="form-group">
                  <button
                    @submit.prevent="EditRatingPoint"
                    class="mt-2 btn btn-successs btn-lg float-right"
                  >{{ $store.getters.GET_SUBMIT}}</button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </template>
  </Master>
</template>

<script>
import Master from "../../layouts/Master";
import axios from "axios";
import Swal from "sweetalert2";
// import "../../layouts/authheader";

export default {
  name: "EditRatingPoint",
  components: { Master },
  data() {
    return {
      name: null,
      value: null,

      department_error_data: {
        name: null,
        value: null,
      },
    };
  },
  methods: {
    getRatingPoint: function () {
      axios.get(`rating-points/${this.$route.params.id}/`).then(
        (response) => {
          this.name = response.data.name;
          this.value = response.data.value;
        },
        (response) => {
          console.log("----", response);
        }
      );
    },

    EditRatingPoint() {
      axios
        .put("rating-points/" + this.$route.params.id + "/", {
          name: this.name,
          value: this.value,
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            text:  this.$store.getters.GET_UPDATED +' '+this.$store.getters.GET_SUCCESSFULLY  +"...",
          }).then((response) => {
            this.$router.push({ name: "RatingPointList" });
            console.log(response);
          });
          console.log(response);
        })
        .catch((error) => {
          this.department_error_data = error.response.data;
          console.log("--++", error.response);
        });
    },
  },
  created() {
    this.getRatingPoint();
  },
};
</script>

<style scoped>
</style>