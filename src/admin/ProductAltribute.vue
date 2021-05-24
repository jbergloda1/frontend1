<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <b-nav-form>
            <b-form-input
              class="mr-sm-2"
              v-model="search"
              placeholder="Search"
              @keyup="searchUnit"
            ></b-form-input>
          </b-nav-form>
          <card
            class="strpied-tabled-with-hover"
            body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              
              
              <table class="table">
                <thead>
                  <slot name="columns">
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Quantity</th>
                      <th>Image</th>
                      <th>Price</th>
                      <th>Status</th>
                      <th>Action</th>
                    </tr>
                  </slot>
                </thead>
                
                <tbody>
                  <tr
                    v-for="product in products.data"
                    v-bind:key="product.id"
                    v-if="product.status == 1"
                  >
                    <td>{{ product.id }}</td>
                    <td>{{ product.name }}</td>
                    <td>{{ product.amount }}</td>
                    <td>
                      <img
                        v-bind:src="
                          'http://127.0.0.1:8000/uploads/product/' + product.img
                        "
                        width="100"
                        alt="product"
                      />
                    </td>
                    <td>{{ product.export_price }}</td>
                    <td>
                      <b-badge variant="success">Đang kinh doanh</b-badge>
                    </td>
                    <td>
                     <b-button
                        variant="outline-info"
                        v-b-modal.modal-center
                        @click="show(product.id)"
                        >Show</b-button
                      >                      
                    </td>
                  </tr>
                </tbody>
                 <b-modal
                  size="xl"
                  id="modal-center"
                  ref="modal"
                  title="THÔNG TIN CHI TIẾT HÓA ĐƠN"
                  @show="resetModal"
                  @hidden="resetModal"
                  @ok="handleOk"
                >
                  <table class="table">
                    <thead>
                      <tr v-bind:key="id">
                        <th>
                          <h4><b>ten san pham {{product.id}}</b></h4>
                        </th>
                      </tr>
                    </thead>
                  </table>
                </b-modal>
              </table>
            </template>
            
          </card>
          <card
            class="strpied-tabled-with-hover"
            body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <h4 class="card-title">Sản phẩm ngừng kinh doanh</h4>
              <table class="table">
                <thead>
                  <slot name="columns">
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Quantity</th>
                      <th>Image</th>
                      <th>Price</th>
                      <th>Status</th>
                      <th>Action</th>
                    </tr>
                  </slot>
                </thead>
                <tbody>
                  <tr
                    v-for="product in products.data"
                    v-bind:key="product.id"
                    v-if="product.status == 2"
                  >
                    <td>{{ product.id }}</td>
                    <td>{{ product.name }}</td>
                    <td>{{ product.amount }}</td>
                    <td>
                      <img
                        v-bind:src="
                          'http://127.0.0.1:8000/uploads/product/' + product.img
                        "
                        width="100"
                        alt="product"
                      />
                    </td>
                    <td>{{ product.export_price }}</td>
                    <td>
                      <b-badge variant="danger">Ngừng kinh doanh</b-badge>
                    </td>
                    <td>
                      <a href="#" @click="clickKinhDoanh(product.id)">
                        <i class="fa fa-trash"></i>
                      </a>

                      <!-- <a href="#" >
                        <i class="fa fa-trash"></i>
                      </a> -->
                    </td>
                  </tr>
                </tbody>
              </table>
            </template>
          </card>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
import Swal from "sweetalert2";
Vue.use(VueAxios, axios);
export default {
  data() {
    return {
      search: "",
      listSupp: [],
      listCate: [],
      listSize: [],
      listColor: [],
      products: [],
      image: null,
      formadd: {
        id: "",
        name: "",
        note: "",
        import_price: "",
        sale: "",
        supplier_id: "",
        category_id: ""
      },
      img: "",
      amount: [],
      color_id: [],
      size_id: [],
      // formedit: {
      //   name: "",
      //   amount: "",
      //   img: "",
      //   note: "",
      //   import_price: "",
      //   sale: "",
      //   supplier_id: "",
      //   color_id: "",
      //   size_id: "",
      //   Quantity: ""
      // },

      apartments: [
        {
          color_id: "",
          size_id: "",
          amount: ""
        }
      ]
    };
  },
  computed: {
    result: function() {
      return this.formadd.import_price * ((100 - this.formadd.sale) / 100);
    }
  },
  mounted() {},
  created() {
    this.getItem();
    this.getSup();
    this.getSize();
    this.getColor();
    this.getCate();
  },
  methods: {
    searchUnit() {
      axios
        .get("http://127.0.0.1:8000/api/product?name=" + this.search)
        .then(response => {
          this.products = response.data;
        });
    },
    show(id) {
      var self = this;
      this.formadd.id = id;
      axios.get("http://127.0.0.1:8000/api/product/"+id).then(res => {
        self.name = res.data.data.name;
        self.note = res.data.data.note;
        self.import_price = res.data.data.import_price;
        self.sale = res.data.data.sale;
        self.supplier_id = res.data.data.supplier_id;
        self.category_id = res.data.data.category_id;
      });
    },
    onImageChange(e) {
      let files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      let formData = new FormData();
      this.createImage(files[0]);
      formData.append("img", e.target.files[0]);
    },
    createImage(file) {
      let reader = new FileReader();
      let vm = this;
      reader.onload = e => {
        vm.image = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    getSup() {
      var self = this;
      Vue.axios
        .get("http://127.0.0.1:8000/api/supplier")
        .then(function(resp) {
          self.listSupp = resp.data;
        })
        .catch(function(error) {
          console.log("Loi:", error);
        });
    },
    getCate() {
      var self = this;
      Vue.axios
        .get("http://127.0.0.1:8000/api/category")
        .then(function(resp) {
          self.listCate = resp.data;
          console.log("Data category:", resp.data.data);
        })
        .catch(function(error) {
          console.log("Loi:", error);
        });
    },
    getSize() {
      var self = this;
      Vue.axios
        .get("http://127.0.0.1:8000/api/size")
        .then(function(resp) {
          self.listSize = resp.data;
        })
        .catch(function(error) {
          console.log("Loi size:", error);
        });
    },
    getColor() {
      var self = this;
      Vue.axios
        .get("http://127.0.0.1:8000/api/color")
        .then(function(resp) {
          self.listColor = resp.data;
          console.log("Data color:", resp.data.data);
        })
        .catch(function(error) {
          console.log("Loi color:", error);
        });
    },
    clickDelete(id) {
      this.formadd.id = id;
      Swal.fire({
        title: "Bạn chắc chắn ngừng kinh doanh sản phẩm này?",
        text: "You won't be able to revert this!",
        showCancelButton: true,
        confirmButtonColor: "#d33",
        cancelButtonColor: "#3085d6",
        confirmButtonText: "Yes, delete it!"
      }).then(result => {
        if (result.value) {
          axios
            .delete(`http://127.0.0.1:8000/api/product/` + id)
            .then(() => {
              Swal.fire(
                "Đã ngừng kinh doanh!",
                "Đã ngừng kinh doanh thành công.",
                "success"
              );
              this.getItem();
              console.log("Thành công");
            })
            .catch(error => {
              this.getItem();
              Swal.fire("Failed!", error.message, "warning");
              console.log("Lỗi", error);
            });
        }
      });
    },
    clickKinhDoanh(id) {
      this.formadd.id = id;
      Swal.fire({
        title: "Bạn chắc chắn muốn kinh doanh sản phẩm này?",
        text: "You won't be able to revert this!",
        showCancelButton: true,
        confirmButtonColor: "#d33",
        cancelButtonColor: "#3085d6",
        confirmButtonText: "Ok!"
      }).then(result => {
        if (result.value) {
          axios
            .delete(`http://127.0.0.1:8000/api/product/` + id)
            .then(() => {
              Swal.fire(
                "Đã kinh doanh!",
                "Đã kinh doanh thành công.",
                "success"
              );
              this.getItem();
              console.log("Thành công");
            })
            .catch(error => {
              this.getItem();
              Swal.fire("Failed!", error.message, "warning");
              console.log("Lỗi", error);
            });
        }
      });
    },
    Count() {},
    addNewPartment() {
      this.apartments.push({
        color_id: "",
        size_id: "",
        amount: ""
      });
    },
    getItem() {
      var self = this;
      Vue.axios
        .get("http://127.0.0.1:8000/api/product")
        .then(function(resp) {
          self.products = resp.data;
        })
        .catch(function(error) {
          console.log("Loi:", error);
        });
    },
    AddProduct() {
      let formData = new FormData();

      formData.append("name", this.formadd.name);
      formData.append("img", this.img);
      formData.append(
        "export_price",
        this.formadd.import_price * ((100 - this.formadd.sale) / 100)
      );
      formData.append("sale", this.formadd.sale);
      formData.append("supplier_id", this.formadd.supplier_id);
      formData.append("category_id", this.formadd.category_id);
      formData.append("color_id", this.color_id);
      formData.append("size_id", this.size_id);
      formData.append("amount", this.amount);
      // let arr=[{"amount": this.amount},{"color_id": this.color_id},{"size_id": this.size_id}]
      // formData.append(arr);
      formData.append("note", this.formadd.note);
      formData.append("import_price", this.formadd.import_price);
      const config = {
        headers: { "Content-type": "multipart/form-data" }
      };
      console.log("NEw product", formData);
      axios
        .post(`http://127.0.0.1:8000/api/product`, formData, config)
        .then(res => {
          this.getItem();
          console.log("Thành công");
          Swal.fire("Đã thêm!", "Thêm user thành công.", "success");
        })
        .catch(error => {
          this.getItem();
          Swal.fire("Failed!", error, "warning");
        });
    }
  }
};
</script>
<style>
.modal.show .modal-dialog {
  transform: translate(0, 50%);
  padding: 2px 0px;
}
.editcategory {
  border: none;
  padding: 4px 12px;
  margin-left: 1px;
  background-color: #0e6de9;
}
.editcategory:hover {
  color: #212529;
  background-color: #0e6de9;
}
.editcategory:active {
  color: #212529;
  background-color: #189ce9 !important;
}
.fixed {
  padding: 1px 1px;
}
.img1 {
  width: 30%;
  margin: auto;
  display: block;
  margin-bottom: 10px;
}
/* .b-submit {
  background-color: red;
  color: black;
  padding: 1rem;
  cursor: pointer;
} */
</style>
