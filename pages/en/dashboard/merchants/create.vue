<template>
  <div class="en-merchants">
    <div :class="mainSide" class="en-merchants-content">
      <merchant />
    </div>
  </div>
</template>
<script>
  // import merchantForms from "~/components/en/Dashboard/merchantForms.vue"
  import merchant from "~/components/en/Forms/merchant/index"

  export default {
    middleware: ["check-auth", "auth"],
    components: {
      // merchantForms,
      merchant
    },
    head() {
      return {
        titleTemplate: '%s - Merchants',
      }
    },
    props: {
      mainSide: {
        type: String,
        default: null,
      },
    },
    layout: "adminlayout",
  }
</script>

<style lang="scss" scoped>
  .en-merchants {
    width: 100%;
    display: flex;
    flex-flow: row;
    align-items: center;
    justify-content: center;
    &-content {
      margin-top: 5%;
      width: 100%;
      display: flex;
      flex-flow: row;
      justify-content: center;
    }
  }
</style>
