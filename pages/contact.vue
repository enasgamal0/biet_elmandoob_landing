<template>
  <section class="contact">

    <div class="all_content">
      <div class="container">

        <div class="row justify-content-center">

          <div class="col-lg-10">

            <div class="box_contact">
              <div class="row">

                <div class="all_info" data-aos="fade-left">

                  <div class="head text-center">
                    <h3 class="main_head text-center">{{ $t('contact.title') }}</h3>
                  </div>

                  <ValidationObserver v-slot="{ invalid }" ref='observer'>
                    <form action="" @submit.prevent="sendData">

                      <ValidationProvider rules="required" :name="$t('contact.name')" v-slot="{ errors }">
                        <div class="form-group">
                          <input type="text" v-model="form.name" :placeholder="$t('contact.name')" />
                          <span class="validation_message">{{ errors[0] }}</span>
                        </div>

                      </ValidationProvider>
                      <ValidationProvider rules="required|email" :name="$t('contact.email')" v-slot="{ errors }">
                        <div class="form-group">
                          <input type="email" v-model="form.email" :placeholder="$t('contact.email')" />
                          <span class="validation_message">{{ errors[0] }}</span>
                        </div>
                      </ValidationProvider>

                      <ValidationProvider rules="required" :name="$t('contact.phone')" v-slot="{ errors }">
                        <div class="form-group">
                          <input type="text" v-model="form.mobile" :placeholder="$t('contact.phone')" />
                          <span class="validation_message">{{ errors[0] }}</span>
                        </div>

                      </ValidationProvider>

                      <ValidationProvider rules="required" :name="$t('user.select_option')" v-slot="{ errors }">
                        <div class="form-group">
                          <select v-model="form.message_type">
                            <option value="" selected disabled>{{ $t('user.select_option') }}</option>
                            <option v-for="(option, index) in messageTypes" :key="'l' + index" :value="option.value">{{
                              option.name }}
                            </option>
                          </select>
                        </div>
                      </ValidationProvider>

                      <ValidationProvider rules="required" :name="$t('contact.message')" v-slot="{ errors }">
                        <div class="form-group">
                          <textarea v-model="form.message" :placeholder="$t('contact.message')"></textarea>
                          <span class="validation_message">{{ errors[0] }}</span>
                        </div>
                      </ValidationProvider>

                      <div class="form-group d-flex">
                        <button class="send_btn main--btn" aria-label="send" title="send"
                          type="submit">{{
                            $t('contact.send') }}</button>
                      </div>

                    </form>
                  </ValidationObserver>

                </div>


              </div>
            </div>

          </div>

        </div>

      </div>
    </div>

  </section>
</template>

<script>

import { ValidationProvider, ValidationObserver } from "vee-validate";

export default {

  name: "contactPage",

  layout: 'staticContent',


  // define all properties

  components: {
    ValidationProvider,
    ValidationObserver
  },


  data() {
    return {

      form: {
        name: "",
        email: "",
        mobile: "",
        message: "",
        message_type: ""
      },


    }
  },

  computed: {

    messageTypes() {
      return [
        {
          id: 1,
          name: this.$t("STATUS.request"),
          value: "Request",
        },
        {
          id: 2,
          name: this.$t("STATUS.suggestion"),
          value: "Suggestion",
        },
        {
          id: 3,
          name: this.$t("STATUS.inquiry"),
          value: "Inquiry",
        },
        {
          id: 4,
          name: this.$t("STATUS.complaint"),
          value: "Complaint",
        },
        {
          id: 5,
          name: this.$t("STATUS.other"),
          value: "Another",
        },
        // {
        //   id: 6,
        //   name: this.$t("STATUS.all"),
        //   value: null,
        // },
      ];
    },

  },

  //  when component load

  mounted() {

    window.scrollTo(0, 0);
    this.$nextTick(() => {
      window.scrollTo(0, 0);
    });
  },


  // All methods and logic

  methods: {
    // send data from contact form

    async sendData() {

      try {
        await this.$axios.$post('static-pages/contact-us', this.form).then(response => {

          this.form.name = '';
          this.form.email = '';
          this.form.mobile = '';
          this.form.message = '';

          this.$refs.observer.reset();

          console.log(response)

          this.$swal.fire({
            position: 'center',
            type: 'success',
            text: `${this.$t("validations.sent_success")}`,
            showConfirmButton: false,
            timer: 3000
          })

          this.$router.push(this.localePath({ path: "/" }));


        }).catch(error => {

          this.$swal.fire({
            position: 'center',
            type: 'error',
            text: `${error.response.data.message || error.response.data.messages}`,
            showConfirmButton: false,
            timer: 3000
          })

          console.log(error)

        })
      } catch (error) {
        console.log('try catch =>', error);
      }

    },

  }
}
</script>

<style lang="scss" scoped>
.validation_message {
  font-size: 13px;
  color: red;
}
</style>
