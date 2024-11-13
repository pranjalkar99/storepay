<template>
    <div class="pa-4 text-center">
      <!-- Coupon Dialog Trigger -->
      <v-dialog v-model="couponDialog" width="auto" scrollable>
        <template v-slot:activator="{ props: activatorProps }">
          <v-btn
            color="brown"
            prepend-icon="mdi-tag"
            text="Apply Coupon"
            variant="outlined"
            v-bind="activatorProps"
          ></v-btn>
        </template>
  
        <!-- Coupon Selection Dialog -->
        <template v-slot:default="{ isActive }">
          <v-card title="Select a Coupon">
            <v-divider class="mt-3"></v-divider>
  
            <v-card-text class="px-4" style="height: 300px;">
              <!-- Coupon Options as Radio Buttons -->
              <v-radio-group
                v-model="selectedCoupon"
                messages="Select a coupon to apply a discount"
                column
              >
                <v-radio label="15% Off - Coupon15" value="coupon15"></v-radio>
                <v-radio label="10% Off - Coupon10" value="coupon10"></v-radio>
                <v-radio label="5% Off - Coupon5" value="coupon5"></v-radio>
              </v-radio-group>
            </v-card-text>
  
            <v-divider></v-divider>
  
            <!-- Dialog Actions -->
            <v-card-actions>
              <v-btn text="Close" @click="couponDialog = false"></v-btn>
              <v-spacer></v-spacer>
              <v-btn color="primary" text="Apply Coupon" @click="applyCoupon"></v-btn>
            </v-card-actions>
          </v-card>
        </template>
      </v-dialog>
  
      <!-- Cart Summary -->
      <div class="mt-4">
        <v-row>
          <v-col>Cart Total:</v-col>
          <v-col class="text-right">{{ cartTotal | currency }}</v-col>
        </v-row>
        <v-row>
          <v-col>Discount:</v-col>
          <v-col class="text-right">{{ discountAmount | currency }}</v-col>
        </v-row>
        <v-row>
          <v-col>Sub Total:</v-col>
          <v-col class="text-right">{{ subTotal | currency }}</v-col>
        </v-row>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        couponDialog: false,
        selectedCoupon: '',
        cartTotal: 100.00, // Example cart total
        discountAmount: 0,
      };
    },
    computed: {
      subTotal() {
        return (this.cartTotal - this.discountAmount).toFixed(2);
      },
    },
    methods: {
      applyCoupon() {
        // Apply discount based on selected coupon
        switch (this.selectedCoupon) {
          case 'coupon15':
            this.discountAmount = this.cartTotal * 0.15;
            break;
          case 'coupon10':
            this.discountAmount = this.cartTotal * 0.10;
            break;
          case 'coupon5':
            this.discountAmount = this.cartTotal * 0.05;
            break;
          default:
            this.discountAmount = 0;
        }
        this.couponDialog = false; // Close dialog
      },
    },
    filters: {
      currency(value) {
        return `$${parseFloat(value).toFixed(2)}`;
      },
    },
  };
  </script>
  
  <style scoped>
  .text-center {
    color: #ffa500;
  }
  </style>
  