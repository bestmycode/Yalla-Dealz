<template>
  <div>
    <!-- <editMerchantForm :merchant="merchant"/> -->
    <merchant />
  </div>
</template>

<script>
  import editMerchantForm from "~/components/en/Forms/editMerchantForm.vue"
  import merchant from "~/components/en/Forms/merchant/index"

  export default {
    layout: "adminlayout",
    head() {
      return {
        titleTemplate: '%s - Deals',
      }
    },
    components: {
      editMerchantForm,
      merchant
    },
    data() {
      return {
        merchant: [],
      }
    },
    mounted() {
      // this.getMerch()
    },
    methods: {
      getMerch() {
        let config = {
          headers: {
            authorization: this.$store.getters.token,
            "Content-Type": `application/json`,
          },
        }
        this.$axios.get("/merchants/"+this.$route.params.idEdit , config).then((res) => {
          this.merchant  = [res.data.merchant , res.data.payment]
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .en-deals {
    display: flex;
    justify-content: center;

    &-content {
      margin-top: 10%;
      width: 90%;
    }
  }
</style>
