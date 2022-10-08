<script setup>
import {onMounted, ref, computed} from "vue";
import makeupImage from "@/assets/img/makeup.jpeg";
import Product from "@/components/Product.vue";

const isCartOpen = ref (false);
const products = ref([]);

onMounted(() => { 
  products.value = [
     {
       id: 1,
       img: makeupImage,
       name: "Red lipstick",
       price: 16,
       quantity: 2
     },
     {
       id: 2,
       img: makeupImage,
       name: "Red lipstick",
       price: 16,
       quantity: 1
     },
     {
       id: 3,
       img: makeupImage,
       name: "Red lipstick",
       price: 16,
       quantity: 1
     },
     {
       id: 4,
       img: makeupImage,
       name: "Red lipstick",
       price: 16,
       quantity: 1
     }
   ];
});

const numCartItems = computed(() => 
  products.value.reduce((numItems, product) => numItems + product.quantity, 0)
);

const cartTotal = computed(() =>
  products.value.reduce((total, product) => total + product.price * product.quantity, 0)
);
const showCart= () => {
  isCartOpen.value =true;
};

const hideCart = () => {
  isCartOpen.value = false;
};

const removeItem = ($event) => {
  const productId = $event;

  products.value= products.value.filter((product) => product.id !== productId )
};

const incrementQuantity = ($event) => {
  const productId = $event;

  const productIndex = products.value.findIndex((product) => product.id === productId);
  products.value[productIndex].quantity++;
};

const decrementQuantity = ($event) => {
  const productId = $event;

  const productIndex = products.value.findIndex((product) => product.id === productId);
  if(products.value[productIndex].quantity === 1)
    removeItem(productId);
  else products.value[productIndex].quantity--;
};

</script>


<template>
 <!-- cart -->
    <button id="cart-btn" @click="showCart">
      <i class="fa-brands fa-opencart fa-xl"></i>
    </button>
    <!-- cart sidebar -->
    <div id="cart-sidebar" class="cart-sidebar" :class="{show: isCartOpen}">
      <!-- header -->
      <div class="cart-header-padding">
        <div class="cart-header">
          <p>
            <span class="title">cart</span>
             <span class="items">
             (<span>{{numCartItems}}</span>
              {{ numCartItems === 1 ? "item" : "items" }})</span>
          </p>
          <button id="close-btn" @click="hideCart">
            <i class="fa-solid fa-xmark fa-xl"></i>
          </button>
        </div>
      </div>
      <!-- products -->
      <div class="cart-products">
        <Product
         v-for="product in products" 
         :key="product.id" 
         :product="product"
         @remove-item="removeItem"
         @increment-quantity="incrementQuantity"
         @decrement-quantity="decrementQuantity" />       
      </div>
      <!-- footer -->
      <div class="cart-footer">
        <div class="total">
          <p>total</p>
          <p>${{cartTotal}}</p>
        </div>
        <a href="#" class="checkout-link">checkout</a>
      </div>
    </div>

</template>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  height: 100%;
}

body {
  font-family: "Poppins", sans-serif;
  letter-spacing: 1px;
  padding: 16px 24px;
}

button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}

a {
  text-decoration: none;
}

/* sidebar */
.cart-sidebar {
  height: 100%;
  width: 420px;
  background-color: white;
  position: fixed;
  top: 0;
  left: -100%;
  box-shadow: 5px 0px 10px rgba(0, 0, 0, 0.1);
  padding-top: 20px;
  display: flex;
  flex-direction: column;
  transition: left 0.4s;
}

.cart-sidebar.show {
  left: 0;
}

/* header */
.cart-header-padding {
  padding-left: 30px;
  padding-right: 30px;
}

.cart-header {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #e8e8e8;
  padding-bottom: 15px;
  margin-bottom: 20px;
}

.title {
  font-size: 14px;
  text-transform: uppercase;
  font-weight: 700;
}

.items {
  font-size: 10px;
}

/* products */
.cart-products {
  flex-grow: 1;
  overflow-y: auto;
  padding: 0 30px 10px;
}

/* footer */
.cart-footer {
  padding: 0 30px 20px;
  box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.2);
}

.total {
  font-size: 14px;
  text-transform: uppercase;
  font-weight: 700;
  display: flex;
  justify-content: space-between;
  padding-top: 20px;
  padding-bottom: 20px;
}

.checkout-link {
  font-size: 12px;
  font-weight: 700;
  background-color: black;
  text-transform: uppercase;
  display: inline-block;
  width: 100%;
  border-radius: 3px;
  color: white;
  text-align: center;
  padding: 13px 5px;
}
</style>
