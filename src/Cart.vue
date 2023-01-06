<template>
  <div class="container mt-5">
    <div class="row">
      <h2 align="center">Workshop #4 - ตะกร้าสินค้า</h2>
    </div>
    <!-- btn Model -->
    <div class="d-flex justify-content-lg-end justify-content-md-center">
      <button
        class="btn btn-lg btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#myModel"
      >
        <i class="fa-solid fa-cart-shopping"></i> ตะกร้าสินค้า({{
          itemInCart.length
        }})
      </button>
    </div>

    <!-- My Model -->
    <div
      class="modal fade"
      id="myModel"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-xl">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">ตะกร้าสินค้า</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <table class="table">
              <thead>
                <tr>
                  <th>สินค้า</th>
                  <th>ราคาต่อชิ้น</th>
                  <th class="text-center">จำนวน</th>
                  <th>ราคารวม</th>
                </tr>
              </thead>
              <tbody>
                <tr v-if="itemInCart.length <= 0">
                  <td class="text-center" colspan="4">
                    สินค้าในตะกร้าของคุณยังว่างอยู่
                  </td>
                </tr>
                <tr v-else v-for="(item, index) in itemInCart" :key="index">
                  <td>
                    <div class="row align-items-center">
                      <div class="col-md-3">
                        <img :src="item.image" width="100" height="100" />
                      </div>
                      <div class="col-md-9">{{ item.name }}</div>
                    </div>
                  </td>
                  <td width="20%" class="align-middle">
                    {{ item.price + "฿" }}
                  </td>
                  <td width="15%" class="align-middle">
                    <div class="input-group">
                      <span
                        class="input-group-text custom-btn-cart"
                        @click="decrementCart(item.id)"
                      >
                        -
                      </span>
                      <input
                        type="number"
                        class="form-control text-center w-20"
                        v-model="item.qty"
                        min="1"
                        ref="InputTotal"
                        :data-item-id="item.id"
                      />
                      <span
                        class="input-group-text custom-btn-cart"
                        @click="incrementCart(item.id)"
                      >
                        +
                      </span>
                    </div>
                  </td>
                  <td class="align-middle">
                    {{ (item.price * item.qty).toLocaleString() + "฿" }}
                  </td>
                </tr>
              </tbody>
              <tfoot>
                <tr>
                  <th colspan="3" class="text-center">ราคารวมทั้งหมด</th>
                  <th>{{ totalPrice().toLocaleString() + "฿" }}</th>
                </tr>
              </tfoot>
            </table>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              ปิดตะกร้าสินค้า
            </button>
            <button type="button" class="btn btn-primary">ชำระเงิน</button>
          </div>
        </div>
      </div>
    </div>
    <hr />
    <div class="row">
      <div class="col-md-4 mb-3" v-for="(item, index) in products" :key="index">
        <div class="card h-100">
          <img
            :src="item.image"
            class="card-img-top"
            width="413"
            height="450"
          />
          <div class="card-body justify-centent-between">
            <h5 class="card-title">{{ item.name }}</h5>
            <h3 class="card-text">{{ item.price.toLocaleString() + "฿" }}</h3>
          </div>
          <div class="card-footer text-center">
            <button
              type="button"
              class="btn btn-success"
              @click="addInCart(item)"
            >
              หยิบลงตะกร้า
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CartComponent",
  data() {
    return {
      itemInCart: [],
      products: [
        {
          id: 1,
          name: "เครื่องผลิตออกซิเจน Yuwell รุ่น 7F-3W ขนาด 3 ลิตร ",
          price: 13900,
          image:
            "https://cx.lnwfile.com/_/cx/_raw/wz/75/df.jpg",
          qty: 1,
        },
        {
          id: 2,
          name: "หนังสือ พัฒนาเว็บแอปพลิเคชั่นด้วย JavaScript",
          price: 350,
          image:
            "https://storage.naiin.com/system/application/bookstore/resource/product/201604/191081/1000184623_front_XXXL.jpg?imgname=%E0%B8%9E%E0%B8%B1%E0%B8%92%E0%B8%99%E0%B8%B2%E0%B9%80%E0%B8%A7%E0%B9%87%E0%B8%9A%E0%B9%81%E0%B8%AD%E0%B8%9A%E0%B8%9E%E0%B8%A5%E0%B8%B4%E0%B9%80%E0%B8%84%E0%B8%8A%E0%B8%B1%E0%B9%88%E0%B8%99%E0%B8%94%E0%B9%89%E0%B8%A7%E0%B8%A2-JavaScript",
          qty: 1,
        },
        {
          id: 3,
          name: "Pizza",
          price: 350,
          image:
            "https://wallpaperbat.com/img/268475-cheese-pizza-wallpaper-mobile-free-download-good-pizza-food.jpg",
          qty: 1,
        },
        {
          id: 4,
          name: "Pie",
          price: 330,
          image:
            "https://wallpapersmug.com/large/f81004/Strawberry-pie-food.jpg",
          qty: 1,
        },
        {
          id: 5,
          name: "Fried Chicken",
          price: 80,
          image:
            "https://images.unsplash.com/photo-1626645738196-c2a7c87a8f58?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8ZnJpZWQlMjBjaGlja2VufGVufDB8fDB8fA%3D%3D&w=1000&q=80",
          qty: 1,
        },
        {
          id: 6,
          name: "French Fry",
          price: 60,
          image:
            "https://images.unsplash.com/photo-1541592106381-b31e9677c0e5?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80",
          qty: 1,
        },
      ],
    };
  },
  methods: {
    addInCart(item) {
      if (!this.itemInCart.find((ele) => ele.id == item.id)) {
        this.itemInCart.push({
          id: item.id,
          name: item.name,
          price: item.price,
          qty: item.qty,
          image: item.image,
        });
        alert("เพิ่มสินค้าลงตะกร้าแล้ว");
      } else {
        if (
          confirm(
            "คุณมีสินค้านี้อยู่ในแล้วในตะกร้า คุณต้องการเพิ่มจำนวนสินค้านี้ในตะกร้าใช่หรือไม่?"
          )
        ) {
          let foundIndex = this.itemInCart.findIndex(
            (ele) => ele.id == item.id
          );
          this.itemInCart[foundIndex].qty++;
        }
      }
    },
    totalPrice() {
      var sum = 0;
      this.itemInCart.forEach((ele) => {
        sum += ele.price * ele.qty;
      });
      return sum;
    },
    decrementCart(id) {
      let foundIndex = this.itemInCart.findIndex((ele) => ele.id == id);
      if (this.itemInCart[foundIndex].qty <= 1) {
        if (
          confirm(
            "คุณต้องการนำรายการนี้ออกหรือไม่?"
          )
        ) {
          this.itemInCart.splice(foundIndex, 1);
        }
      } else {
        this.itemInCart[foundIndex].qty--;
      }
    },
    incrementCart(id) {
      let foundIndex = this.itemInCart.findIndex((ele) => ele.id == id);
      this.itemInCart[foundIndex].qty++;
    },
  },
};
</script>

<style scoped>
.custom-btn-cart {
  cursor: pointer;
}

input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
