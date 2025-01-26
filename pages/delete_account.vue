<template>
    <div class="static_page_content_wrapper">  
      <div class="container">
        <div class="page_title">
          <h2>{{ terms_title }}</h2>
        </div>
  
        <div class="page_content">
          <p class="item_content" v-html="terms_content"></p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import MainLoader from "~/components/ui/MainLoader.vue";
  export default {
    name: "PrivacyPolicy",
    layout: "staticContent",
    components: {
      MainLoader,
    },
  
    data() {
      return {
        isLoading: false,
        terms_title: '',
        terms_content: ''
      };
    },
  
    methods: {
      async getData() {
        try {
          return await this.$axios.get(`static_pages/delete-account`).then(response => {
            console.log("response", response)
            this.isLoading = true;
            this.terms_content = response.data.data.content;
            this.terms_title = response.data.data.name;
          }).catch(error => {
            console.log(error)
          })
        } catch (error) {
          console.log("catch : " + error)
        }
      }
    },
  
    mounted() {
      this.getData();
    }
  };
  </script>
  
  <style lang="scss" scoped>
  .static_page_content_wrapper {
    padding-block: 30px 40px;
    background-color: #f8f8f8;
  
    .page_title {
      h2 {
        margin-bottom: 20px;
        text-align: center;
        font-size: 35px;
        color: var(--main_theme_clr);
      }
    }
  
    .page_content {
      margin-top: 25px;
  
      .item_title,
      .item_content {
        word-break: break-word;
        word-spacing: 2px;
        line-height: 1.6;
        text-align: start;
      }
  
      .item_title {
        font-size: 22px;
        color: var(--main_theme_clr);
      }
  
      .item_content {
        font-size: 17px;
        color: var(--light_gray_clr);
      }
    }
  }
  </style>
  