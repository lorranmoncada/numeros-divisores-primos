<template>
  <div class="alert-position ml-5">
    <b-alert
      style="width:23%"
      :show="dismissCountDown"
      dismissible
      variant="warning"
      @dismissed="dismissCountDown = 0"
      @dismiss-count-down="countDownChanged"
    >
      Insira um valor maior que 0
    </b-alert>
    <!-- <b-button @click="showAlert" variant="info" class="m-1">
      Show alert with count-down timer
    </b-button> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      dismissSecs: 5,
      dismissCountDown: this.propSeconds
    };
  },
  methods: {
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown;
    },
    showAlert(value) {
      this.dismissCountDown = value;
    }
  }
};
</script>
<style scoped>
.alert-position {
  display: flex !important;
  justify-content: center !important;
}
</style>