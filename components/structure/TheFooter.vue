<template>
  <div id="contact_us_section" class="footer_content_wrapper">
    <div class="container-xl">
      <div class="row justify-content-between">
        <!-- Start:: Logo Wrapper -->
        <div class="col-lg-3 col-xl-2 my-3">
          <div class="logo_wrapper focus_screen m-auto mb-2">
            <img src="~/assets/media/logo/logo1.png" width="120" height="120" alt="Logo" />
          </div>
          <ul class="social_links_list d-flex w-100">
            <li class="social_links_item" v-if="facebook">
              <a :href="facebook" target="_blank" class="icon">
                <i class="fa-brands fa-square-facebook"></i>
              </a>
            </li>
    
            <li class="social_links_item" v-if="linkedIn">
              <a :href="linkedIn" target="_blank" class="icon">
                <i class="fa-brands fa-linkedin-in"></i>
              </a>
            </li>
    
            <li class="social_links_item" v-if="instagram">
              <a :href="instagram" target="_blank" class="icon">
                <i class="fa-brands fa-instagram"></i>
              </a>
            </li>
    
            <li class="social_links_item" v-if="telegram">
              <a :href="telegram" target="_blank" class="icon">
                <i class="fa-brands fa-telegram"></i>
              </a>
            </li>
    
            <li class="social_links_item" v-if="tiktok">
              <a :href="tiktok" target="_blank" class="icon">
                <i class="fa-brands fa-tiktok"></i>
              </a>
            </li>
    
            <li class="social_links_item" v-if="twitter">
              <a :href="twitter" target="_blank" class="icon">
                <img alt="X Twitter" src="~/assets/media/icons/twitter-x.svg"/>
              </a>
            </li>

            <li class="social_links_item" v-if="whatsapp">
              <a :href="`https://wa.me/${whatsapp}`" target="_blank" class="icon">
                <i class="fa-brands fa-whatsapp"></i>
              </a>
            </li>
    
            <!-- <li class="social_links_item" v-if="email">
              <a :href="`mailto:${email}`" class="icon">
                <i class="fa fa-envelope"></i>
              </a>
            </li> -->
    
            <li class="social_links_item" v-if="snapchat">
              <a :href="snapchat" target="_blank" class="icon">
                <i class="fab fa-snapchat-square"></i>
              </a>
            </li>
          </ul>
        </div>
        <!-- End:: Logo Wrapper -->

        <!-- Start:: Routes Wrapper -->
        <div class="col-lg-5 col-xl-3 d-flex justify-content-center my-3">
          <ul class="footer_routes_list px-5">
            <li class="footer_route">
              <button style="text-align: start;" class="active" @click="scrollToSection('hero_section')"> {{ $t("nav.home") }} </button>
            </li>

            <li class="footer_route">
              <button style="text-align: start;" @click="scrollToSection('about_section')"> {{ $t("nav.about") }} </button>
            </li>

            <li class="footer_route">
              <button style="text-align: start;" @click="scrollToSection('app_screens_section')"> {{ $t("nav.screen") }} </button>
            </li>

            <li class="footer_route">
              <button style="text-align: start;" @click="scrollToSection('download_app_section')"> {{ $t("nav.download") }} </button>
            </li>
            <li class="footer_route">
              <button style="text-align: start;">
                <nuxt-link :to="localePath('/contact')">
                  {{ $t('nav.contact') }}
                </nuxt-link>
              </button>
            </li>
            <li class="footer_route">
              <button style="text-align: start;">
                <nuxt-link :to="localePath('/delete_account')">
                  {{ $t('nav.delete_account') }}
                </nuxt-link>
              </button>
            </li>
          </ul>
        </div>
        <!-- End:: Routes Wrapper -->

        <!-- Start:: Contact Info Wrapper -->
        <div class="col-lg-4 col-xl-5 d-flex justify-content-between flex-column flex-lg-row my-3">
          <ul class="contact_info_list">
            <li class="contact_info_item" v-if="email">
              <span class="icon">
                <i class="fa-regular fa-envelope"></i>
              </span>

              <a :href="'mailto:' + email"><span class="value"> {{ email }}
                </span></a>
            </li>

            <li class="contact_info_item" v-if="phone" v-for="phone_num in phone">
              <span class="icon">
                <i class="fa-solid fa-phone"></i>
              </span>
              <a :href="'tel:' + phone_num"><span class="value"> {{ phone_num }}
              </span></a>
            </li>

            <li class="contact_info_item" v-if="address">
              <span class="icon">
                <i class="fa-solid fa-location-dot"></i>
              </span>
              <span class="value"> {{ address }}</span>
            </li>
          </ul>
        </div>
        
        <!-- End:: Contact Info Wrapper -->
      </div>
      
    </div>

    <div class="copyrights_contet">
      <h6> {{ $t('copyWrite') }} </h6>
    </div>
  </div>
</template>

<script>
export default {
  name: "TheFooter",
  data() {
    return {
      instagram: '',
      telegram: '',
      facebook: '',
      linkedIn: '',
      snapchat: '',
      whatsapp: '',
      email: '',
      tiktok: '',
      twitter: '',
      phone: '',
      address: '',
    }
  },
  methods: {
    // START:: SCROLL TO SECTION
    scrollToSection(section_id) {
      if (section_id == 'contact_us_section') {
        this.$router.push(this.localePath('/contact'))
      } else {
        if (this.$route.path != this.localePath('/')) {
          this.$router.push(this.localePath('/'))
          setTimeout(() => {
            const selected_section = document.querySelector(`#${section_id}`)
            selected_section.scrollIntoView()
          }, 2000)
        } else {
          const selected_section = document.querySelector(`#${section_id}`)
          selected_section.scrollIntoView()
        }
      }
    },
    // END:: SCROLL TO SECTION


    async getData() {
      await this.$axios.get('static-pages/contact-us').then((res) => {
        this.email = res.data.data?.app_contacts?.email;
        this.whatsapp = res.data.data?.app_contacts?.watsApp;
        this.facebook = res.data.data?.app_contacts?.facebook;
        this.linkedIn = res.data.data?.app_contacts?.linkedIn;
        this.instagram = res.data.data?.app_contacts?.instagram;
        this.telegram = res.data.data?.app_contacts?.telegram;
        this.snapchat = res.data.data?.app_contacts?.snapchat;
        this.tiktok = res.data.data?.app_contacts?.tiktok;
        this.twitter = res.data.data?.app_contacts?.twitter;
        this.address = res.data.data?.app_contacts?.address;
        this.phone = res.data.data?.app_contacts?.phones;
      })
    },
  },


  mounted() {
    this.getData();
  }

}
</script>
