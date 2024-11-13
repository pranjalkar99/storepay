<template>
    <v-container>
      <!-- Header with Back Navigation -->
      <v-row >
        <v-btn icon @click="goBack">
          <v-icon>mdi-arrow-left</v-icon>
        </v-btn>
        <h2 class="text-h6">Namaste, You are in Ramu Stores.</h2>
      </v-row>
  
      <!-- Product List -->
      <v-row v-for="(product, index) in products" :key="index" class="mb-4">
        <v-col cols="4">
          <v-avatar size="60">
            <v-img :src="product.image" alt="product image" v-if="product.image"></v-img>
            <svg-icon type="mdi" :path="path"></svg-icon>
          </v-avatar>
        </v-col>
        <v-col cols="8">
          <div>{{ product.name }}</div>
          <div>₹ {{ product.price | currency }}</div>
          <v-row  class="mt-2">
            <v-btn icon @click="decrementQty(index)">
              <v-icon>mdi-minus</v-icon>
            </v-btn>
            <span class="mx-2">{{ product.quantity }}</span>
            <v-btn icon @click="incrementQty(index)">
              <v-icon>mdi-plus</v-icon>
            </v-btn>
            <v-btn icon @click="removeFromWishlist(index)">
              <v-icon>mdi-heart</v-icon>
            </v-btn>
            <v-btn icon @click="removeFromCart(index)">
              <v-icon>mdi-delete</v-icon>
            </v-btn>
          </v-row>
        </v-col>
      </v-row>
  
      <v-row>
  <v-col cols="12" class="d-flex">
    <!-- Coupon Code Input -->
    <v-text-field
      v-model="coupon"
      label="Apply Coupon"
      outlined
      append-inner-icon="mdi-check"
      class="mr-2" 
    ></v-text-field>

    <!-- Apply Coupon Button -->
    <v-btn 
      color="primary" 
      @click="applyCoupon"
      class="ml-auto" 
    >
      Apply Coupon
    </v-btn>
  </v-col>
</v-row>


  
      <!-- Summary Section -->
     <!-- Summary Section -->
<v-divider></v-divider>
<v-row class="pt-4">
  <v-col>Cart Total</v-col>
  <v-col class="text-right">₹ {{ cartTotal | currency }}</v-col>
</v-row>
<v-row>
  <v-col>Tax</v-col>
  <v-col class="text-right">₹ {{ tax | currency }}</v-col>
</v-row>
<v-row>
  <v-col>Discount</v-col>
  <v-col class="text-right">₹ {{ discount | currency }}</v-col>
</v-row>
<v-row>
  <v-col>Sub Total</v-col>
  <v-col class="text-right">₹ {{ subtotal | currency }}</v-col>
</v-row>

  
      <!-- Checkout Button -->
      <v-btn class="my-4" block color="primary" @click="proceedToCheckout">
        Proceed to Checkout
      </v-btn>
    </v-container>
  </template>
  
  <script>
  import SvgIcon from '@jamescoyle/vue-icon';
  import { mdiFoodOutline } from '@mdi/js';
  export default {
    name: "ConfirmPage",
        components: {
          SvgIcon,
        },
    data() {
       
      return {
        path: mdiFoodOutline,
        products: [
          {
            name: "Ashirvaad Atta (1kg)",
            price: 349.0,
            quantity: 1,
          },
          {
            name: "Dabur Aloe Vera Gel",
            price: 196.0,
            quantity: 1,
          },
        ],
        coupon: "FREE50",
        validCoupons: {
      'FREE50': 0.15,  // 15% discount
    },
    discount: 0
      };
    },
    computed: {
  cartTotal() {
    return this.products.reduce((total, product) => total + product.price * product.quantity, 0);
  },
  tax() {
    return ((this.cartTotal - this.discount) * 0.03).toFixed(2); // Tax after discount
  },
  subtotal() {
    return (parseFloat(this.cartTotal) + parseFloat(this.tax) - this.discount).toFixed(2); // Subtotal after discount
  },
},

    methods: {
      goBack() {
        this.$router.go(-1);
      },
      incrementQty(index) {
        this.products[index].quantity++;
      },
      decrementQty(index) {
        if (this.products[index].quantity > 1) {
          this.products[index].quantity--;
        }
      },
      removeFromWishlist(index) {
        // Logic to remove from wishlist
      },
      removeFromCart(index) {
        this.products.splice(index, 1);
      },
      applyCoupon() {
    if (this.validCoupons[this.coupon]) {
        console.log(this.validCoupons[this.coupon]);
      const discountPercentage = this.validCoupons[this.coupon];
      console.log(discountPercentage);
      this.discount = discountPercentage * this.cartTotal;
      alert(`Coupon "${this.coupon}" applied! You get a ${discountPercentage * 100}% discount.`);
    } else {
      alert(`Invalid coupon code!`);
    }
  },
      proceedToCheckout() {
        // Logic for proceeding to checkout
        alert("Proceeding to checkout...");
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
  .text-h6 {
    font-size: 1.2rem;
    color: #ffa500;
  }
  </style>
  