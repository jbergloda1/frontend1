<template>
  <div class="topnavhome">
    <div>
      <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand href="#" to="/login">SHOES STORE</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item to="/home">Home</b-nav-item>
            <b-nav-item to="/shop">Shop</b-nav-item>
            <b-nav-item to="/about">About</b-nav-item>
            <b-nav-item to="/contact">Contact</b-nav-item>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-form>
              <b-form-input
                size="sm"
                class="mr-sm-2"
                placeholder="Search"
              ></b-form-input>
              <b-button size="sm" class="my-2 my-sm-0" type="submit"
                >Search</b-button
              >
            </b-nav-form>
            <b-navbar-nav v-if="mail != null">
              <b-nav-item to="/viewcart">
              Cart (<span class="total-count">{{ quantity_cart }}</span
                >)
                <b-icon
                  icon="cart4"
                  scale="0.75"
                  animation="throb"
                  aria-hidden="true"
                  class="iccart"
                ></b-icon>
              </b-nav-item>
              <b-nav-item-dropdown right>
              <!-- Using 'button-content' slot -->
              <template #button-content>
                <em>{{ mail }}</em>
              </template>
              <b-dropdown-item to="/profile-customer">Profile</b-dropdown-item>
              <b-dropdown-item @click="logout">Sign Out</b-dropdown-item>
            </b-nav-item-dropdown>
            </b-navbar-nav>
             <div v-else>
               <b-nav-item to="/logincustomer">Login</b-nav-item>
              </div>

            
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
export default {
  data() {
    return {
      mail: null,
      quantity_cart: null,
      customer: [],
    };
  },
  created() {
    let token = JSON.parse(window.localStorage.getItem("customer"));
    this.mail = token.email;
  },
  async mounted() {
    this.total();
  },
  methods: {
    logout() {
      this.$store.dispatch("logoutcustomer").then(() => this.$router.go());
    },
    total() {
      var self = this;
      this.customer = JSON.parse(localStorage.getItem("customer"));
      console.log("local:", this.customer.id);
      axios
        .get("http://127.0.0.1:8000/api/total-cart/" + this.customer.id)
        .then(function (resp) {
          self.quantity_cart = resp.data.data;
        })
        .catch(function (error) {
          console.log("Loi cart:", error);
        });
    },
  },
};
</script>

<style scoped>
.item-menu {
  margin-left: 30px;
  /* color: black; */
}
.item-menu :hover {
  color: #01020c;
}
.item-cart {
  float: right;
}
.topnavhome {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  z-index: 1000;
  background-color: white;
}
.cart-bt {
  margin: 15px 15px;
}
</style>
