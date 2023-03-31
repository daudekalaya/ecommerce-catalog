<template>
  <div v-if="isLoading" class="card">
    <div class="contain-product">
      <div class="skeleton">
        <div class="skeleton-left flex1">
          <div class="square"></div>
        </div>
        <div class="skeleton-right flex2">
          <div class="line"></div>
          <div class="line h250"></div>
        </div>
      </div>
    </div>
  </div>
  <div
    v-else
    class="container"
    :class="
      !isProductAvailable
        ? 'bg-gray'
        : item.data.category === 'men\'s clothing'
        ? 'bg-blue'
        : 'bg-pink'
    "
  >
    <img class="patern" src="../assets/bg-pattern.svg" alt="" />
    <div class="contain-product">
      <div v-if="!isProductAvailable" class="product-unavailable-container">
        <div class="overlay">
          <img
            src="../assets/sad-face.svg"
            alt="unavailable product"
          />
        </div>
        <div class="content">
          <p>This product is unavailable to show</p>
          <div class="btn-available">
            <button
              type="button"
              @click="nextItem()"
              class="btn-available-next"
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
      <div v-else>
        <div class="img-product">
          <img :src="item.data.image" :alt="item.data.title" />
        </div>
        <div class="content">
          <div class="wrapper">
            <h3
              :class="
                item.data.category === 'men\'s clothing'
                  ? 'font-navy'
                  : 'font-magenta'
              "
              class="title"
            >
              {{ item.data.title }}
            </h3>
            <div class="sub-title">
              <span>{{ item.data.category }}</span>
              <div class="rating">
                <span>{{ item.data.rating.rate }}/5</span>
                <div class="rating">
                  <span :class="getRatingClass(1)" class="circle"></span>
                  <span :class="getRatingClass(2)" class="circle"></span>
                  <span :class="getRatingClass(3)" class="circle"></span>
                  <span :class="getRatingClass(4)" class="circle"></span>
                  <span :class="getRatingClass(5)" class="circle"></span>
                </div>
              </div>
            </div>
            <p class="desc">
              {{ item.data.description }}
            </p>
          </div>
          <div class="wrapper2">
            <p
              :class="
                item.data.category === 'men\'s clothing'
                  ? 'font-navy'
                  : 'font-magenta'
              "
              class="price"
            >
              ${{ item.data.price }}
            </p>
            <div class="btn">
              <button
                :class="
                  item.data.category === 'men\'s clothing'
                    ? 'bg-navy'
                    : 'bg-magenta'
                "
                class="btn-buy"
              >
                Buy now
              </button>
              <button
                @click="nextItem()"
                :class="
                  item.data.category === 'men\'s clothing'
                    ? 'border-navy font-navy'
                    : 'border-magenta font-magenta'
                "
                class="btn-next"
              >
                Next product
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      isLoading: false,
      index: 0,
      isProductAvailable: false,
      item: [],
    };
  },
  methods: {
    async fetchData() {
      const response = await fetch(
        `https://fakestoreapi.com/products/${this.index}`
      );
      const result = await response.json();
      return result;
    },
    async nextItem() {
      this.isLoading = true;

      if (this.index !== 20) {
        this.index++;
      } else {
        this.index = 1;
      }

      let data = await this.fetchData();
      console.log(data);
      if (
        data.category === "men's clothing" ||
        data.category === "women's clothing"
      ) {
        this.item = { data };
        this.isProductAvailable = true;
      } else {
        this.isProductAvailable = false;
      }

      this.isLoading = false;
    },
    getRatingClass(index) {
      const rating = this.item.data.rating.rate;
      if (rating < index) {
        return this.item.data.category === "men's clothing"
          ? "border-navy"
          : "border-magenta";
      } else {
        return this.item.data.category === "men's clothing"
          ? "bg-navy"
          : "bg-magenta";
      }
    },
  },
  mounted() {
    this.nextItem();
  },
};
</script>
