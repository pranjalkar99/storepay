<template>
  <v-container class="d-flex flex-column align-center justify-center fill-height">
    <!-- Logo Component -->
    <LogoComponent />

    <!-- Welcome Text -->
    <h2 class="text-center">Welcome</h2>

    <form @submit.prevent="onSubmit">
      <!-- Phone Number Input with Validation -->
      <v-text-field
        v-model="phone"
        label="   Phone Number"
        outlined
        :rules="phoneRules"
        maxlength="10"
        counter="10"
        required
      ><svg-icon type="mdi" :path="path"></svg-icon></v-text-field>

      <!-- Submit Button -->
      <v-btn class="me-4 " type="submit" v-if="!otpSent">
        Submit
      </v-btn>
      <v-btn @click="handleReset" v-if="otpSent">
        Clear
      </v-btn>

      <!-- OTP Input Sheet (Shown after phone number is submitted) -->
      <v-sheet
        v-if="otpSent"
        class="py-8 px-6 mx-auto ma-4 text-center"
        elevation="4"
        max-width="500"
        rounded="lg"
        width="100%"
      >
        <h3 class="text-h5">Verification Code</h3>
        <div class="text-subtitle-2 font-weight-light mb-3">
          Please enter the verification code sent to your mobile
        </div>

        <!-- OTP Input -->
        <v-otp-input
          v-model="otp"
          class="mb-8"
          divider="•"
          length="4"
          variant="outlined"
        ></v-otp-input>

        <!-- Resend Code Button -->
        <div class="text-caption">
          <v-btn
            color="primary"
            size="x-small"
            variant="text"
            @click="otp = ''"
          >
            Send New Code
          </v-btn>
        </div>


        <v-btn
        :disabled="loading"
        :loading="loading"
        class="text-none mb-4"
        color="indigo-darken-3"
        size="x-large"
        variant="flat"
        block
        @click="loading = !loading"
      >
        Verify and continue
      </v-btn>
      </v-sheet>

      <!-- Clear Button -->
      
    </form>

    <!-- Bottom Navigation -->
    <BottomNavigation />
  </v-container>
</template>

<script>

import LogoComponent from "./LogoComponent.vue";
import SvgIcon from '@jamescoyle/vue-icon';
import BottomNavigation from "./BottomNavigation.vue";
import { mdiAccount, mdiPhone } from '@mdi/js'

export default {
  name: "WelcomePage",
  components: {
    LogoComponent,
    BottomNavigation,
    SvgIcon
  },
  data() {
    return {
      phone: '',
      otp: '',
      otpSent: false,
      loading: false,
      path: mdiPhone,
      phoneRules: [
        v => !!v || "Phone number is required",
        v => /^[0-9]{10}$/.test(v) || "Phone number must be 10 digits",
      ],
    };
  },
  methods: {
    onSubmit() {
      if (this.phoneRules.every(rule => rule(this.phone) === true)) {
        this.otpSent = true; // Show OTP block if phone number is valid
      } else {
        this.otpSent = false;
      }
    },
    handleReset() {
      this.phone = '';
      this.otp = '';
      this.otpSent = false;
    },
  },
  watch: {
      loading (val) {
        if (!val) return
        console.log('loading...', val);

        setTimeout(() => {
    this.loading = false;
    // After loading is done, navigate to /cart
    this.$router.push('/cart');
  }, 2000); // 2 seconds delay before navigating
} 
    },
};
</script>

<style scoped>
.text-center {
  color: #ffa500;
}
</style>
