<template>
  <!--Section: Block Content-->
  <section class="mb-5 mg">
    <div class="row">
      <div class="col-md-6 mb-4 mb-md-0">
        <div id="mdb-lightbox-ui"></div>

        <div class="mdb-lightbox">
          <div class="row product-gallery mx-1">
            <div class="col-12 mb-0">
              <figure class="view overlay rounded z-depth-1 main-img">
                <img
                  class="img_shop"
                  :src="'http://127.0.0.1:8000/uploads/product/' + product_img"
                  contain
                  width="100%"
                />
              </figure>
            </div>
            <div class="col-12">
              <div class="row">
                <div class="col-3">
                  <div class="view overlay rounded z-depth-1 gallery-item">
                    <img
                      class="img_shop"
                      :src="
                        'http://127.0.0.1:8000/uploads/product/' + product_img
                      "
                      contain
                      width="100%"
                    />
                    <div class="mask rgba-white-slight"></div>
                  </div>
                </div>
                <div class="col-3">
                  <div class="view overlay rounded z-depth-1 gallery-item">
                    <img
                      class="img_shop"
                      :src="
                        'http://127.0.0.1:8000/uploads/product/' + product_img
                      "
                      contain
                      width="100%"
                    />
                    <div class="mask rgba-white-slight"></div>
                  </div>
                </div>
                <div class="col-3">
                  <div class="view overlay rounded z-depth-1 gallery-item">
                    <img
                      class="img_shop"
                      :src="
                        'http://127.0.0.1:8000/uploads/product/' + product_img
                      "
                      contain
                      width="100%"
                    />
                    <div class="mask rgba-white-slight"></div>
                  </div>
                </div>
                <div class="col-3">
                  <div class="view overlay rounded z-depth-1 gallery-item">
                    <img
                      class="img_shop"
                      :src="
                        'http://127.0.0.1:8000/uploads/product/' + product_img
                      "
                      contain
                      width="100%"
                    />
                    <div class="mask rgba-white-slight"></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <h3>{{ product_name }}</h3>
        <div v-if="sale != null">
          <b-badge class="sale" variant="danger"
            >Đang giảm giá {{ sale }}%
          </b-badge>
        </div>
        <div v-if="export_price != null">
          <div class="importpr">
            <strike>{{ formatPrice(import_price) }} Đ</strike>
          </div>
          <div class="salepr">
            <b-badge variant="success"
              >{{ formatPrice(export_price) }} Đ</b-badge
            >
          </div>
        </div>
        <div v-else>
          <div class="importpr">{{ formatPrice(import_price) }} Đ</div>
        </div>
        <p class="pt-1">{{ product_note }}</p>
        <div class="table-responsive">
          <table class="table table-sm table-borderless mb-0">
            <tbody>
              <tr>
                <th class="pl-0 w-25" scope="row">
                  <strong>Nhà cung cấp</strong>
                </th>
                <td>{{ product_supplier }}</td>
              </tr>
              <tr>
                <th class="pl-0 w-25" scope="row"><strong>Category</strong></th>
                <td>{{ product_category }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <hr />
        <div class="table-responsive mb-2">
          <table class="table table-sm table-borderless">
            <tbody>
              <div class="choose">
                <b-form-group label="Chọn màu">
                  <b-form-select class="color" v-model="color">
                    <option
                      v-for="color in listColor.data"
                      :key="color.id"
                      :value="color.id"
                    >
                      {{ color.color }}
                    </option>
                  </b-form-select>
                </b-form-group>
                <b-form-group label="Chọn size">
                  <b-form-select class="size" v-model="size">
                    <option
                      v-for="size in listSize.data"
                      :key="size.id"
                      :value="size.id"
                    >
                      {{ size.size }}
                    </option>
                  </b-form-select>
                </b-form-group>
                <b-form-group label="Số lượng">
                  <b-form-spinbutton
                    class="count"
                    id="demo-sb"
                    v-model="quantity"
                    min="1"
                    max="50"
                  ></b-form-spinbutton>
                </b-form-group>
              </div>
            </tbody>
          </table>
        </div>
        <b-link class="paym" href="#" to="/checkout">
          <button type="button" class="btn btn-primary btn-md mr-1 mb-2">
            Mua ngay
          </button>
        </b-link>
        <b-button
          pill
          @click="addItemToCart()"
          class="btn btn-success btn-md mr-1 mb-2"
        >
          <b-icon icon="cart4" aria-hidden="true" class="iccart"></b-icon>Add to
          cart</b-button
        >
      </div>
    </div>
    <!-- Same Product -->
    <div class="container">
      <div class="home-section-head">
        <h2 class="section-title">HOT DEAL</h2>
      </div>
      <b-row align-h="center">
        <b-col
          cols="4"
          class="place-list"
          v-for="product1 in products.data"
          :key="product1.id"
        >
          <b-card class="cardinfo" overlay text-variant="white">
            <div>
              <img
                class="resize"
                :src="'http://127.0.0.1:8000/uploads/product/' + product1.img"
              />
              <div class="product-coming-soon-2">Mới ra mắt</div>
            </div>

            <div v-if="product1.sale != null">
              <div class="infopr">
                <div v-if="product1.export_price != null">
                  <div class="importpr">
                    <strike>{{ formatPrice(product1.import_price) }}.Đ</strike>
                  </div>
                  <div class="salepr">
                    {{ formatPrice(product1.export_price) }}.Đ
                  </div>
                </div>
                <div v-else>
                  <div class="importpr">
                    {{ formatPrice(product1.import_price) }}.Đ
                  </div>
                </div>
                <p>{{ product1.name }}</p>
              </div>
            </div>
          </b-card>
        </b-col>
      </b-row>
    </div>
    <top-nav-home />
    <content-footer-home />
    <!-- Same Product -->
  </section>
</template>

<script>
import axios from "axios";
import Vue from "vue";
import VueAxios from "vue-axios";
import Swal from "sweetalert2";
import ContentFooterHome from "../components/Footer/ContentFooterHome.vue";
import TopNavHome from "../layout/TopNavHome.vue";
Vue.use(VueAxios, axios);
export default {
  components: {
    TopNavHome,
    ContentFooterHome,
  },
  name: "Preview",
  data() {
    return {
      customer: [],
      product_id: null,
      product_name: "",
      import_price: "",
      export_price: "",
      product_note: "",
      product_category: "",
      product_img: "",
      product_supplier: "",
      sale: "",
      size: null,
      color: null,
      quantity: null,
      suppliers: [],
      listSize: [],
      listColor: [],
      products: [],
      products1: [],
    };
  },
  created() {
    this.getSupp();
    this.getProduct();
    this.getColor();
    this.getSize();
    this.getLatestProduct();
  },
  methods: {
    formatPrice(value) {
      let val = (value / 1).toFixed(0).replace(".", ",");
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
    getLatestProduct() {
      var self = this;
      Vue.axios
        .get("http://127.0.0.1:8000/api/product-new")
        .then((resp) => {
          self.products = resp.data;
          console.log(resp.data);
        })
        .catch(function (error) {
          console.log("Loi:", error);
        });
    },
    addItemToCart() {
      this.customer = JSON.parse(localStorage.getItem("customer"));
      let formData = new FormData();
      formData.append("customer_id", this.customer.id);
      formData.append("product_id", this.product_id);
      formData.append("size_id", this.size);
      formData.append("color_id", this.color);
      formData.append("quantity", this.quantity);
      axios
        .post(
          `http://127.0.0.1:8000/api/add-cart/` + this.customer.id)
        .then((res) => {
          Swal.fire(
            "Đã thêm!",
            "Thêm sản phẩm vào giỏ hàng thành công.",
            "success"
          );
        })
        .catch((error) => {
          Swal.fire("Failed!", error, "warning");
        });
    },
    getProduct() {
      Vue.axios
        .get("http://127.0.0.1:8000/api/product")
        .then((resp) => {
          this.products = resp.data.data;
          console.log(this.products);
        })
        .catch(function (error) {
          console.log("Loi:", error);
        });
    },
  },
  mounted: function () {
    // now we get all the related infomation for the particular product id
    axios
      .get(`http://127.0.0.1:8000/api/product/${this.$route.params.id}`)
      .then((res) => {
        this.product_name = res.data.data.name;
        this.import_price = res.data.data.import_price;
        this.export_price = res.data.data.export_price;
        this.product_note = res.data.data.note;
        this.product_img = res.data.data.img;
        this.product_supplier = res.data.data.supplier_id;
        this.product_category = res.data.data.category_id;
        this.sale = res.data.data.sale;
        console.log(this.product_name);
      }),
      // axios.get(`http://127.0.0.1:8000/api/supplier/${this.product_supplier}`)
      // .then( res => {
      //   this.suppliers = res.data.data;
      //   console.log(this.suppliers);
      // })
      axios
        .get(`http://127.0.0.1:8000/api/product-color/236`)
        .then((res) => {
          this.listColor = res.data;
        })
        .catch(function (error) {
          console.log("Loi:", error);
        });
    axios
      .get(`http://127.0.0.1:8000/api/product-size/236/139`)
      .then((res) => {
        this.listSize = res.data;
      })
      .catch(function (error) {
        console.log("Loi:", error);
      });
  },
};
</script>
 <style scoped>
.mg {
  margin-top: 150px;
}
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.carousel-inner {
  height: 500px;
}
.carousel-item {
  height: 100%;
}
.single-image {
  width: 100%;
  height: 100%;
  object-fit: fill;
}
#demo {
  margin-left: 30px;
  margin-right: 30px;
}
.salepr {
  font-size: 200%;
  color: red;
}
.infopr {
  text-align: center;
}
</style>
