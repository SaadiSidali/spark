<template >
  <v-container>
    <v-row>
      <v-col cols="12" sm="8">
        <v-row class="px-3">
          <p class="title">Livraison</p>
          <v-spacer></v-spacer>
          <p class="title">ادخل معلومات التوصيل</p>
        </v-row>
        <v-divider class="mb-6"></v-divider>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="12" sm="6">
              <v-text-field
                v-model="order.firstName"
                hint="الاسم"
                :rules="nameRules"
                label="Prenom"
                outlined
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                hint="اللقب"
                v-model="order.lastName"
                :rules="nameRules"
                label="Nom"
                outlined
              ></v-text-field>
            </v-col>
          </v-row>
          <v-text-field
            hint="العنوان"
            label="Adress"
            v-model="order.adress"
            :rules="nameRules"
            outlined
          ></v-text-field>

          <v-autocomplete
            :rules="nameRules"
            hint="الولاية"
            outlined
            v-model="order.wilayaCode"
            :items="wilayaList"
            label="Wilaya"
          ></v-autocomplete>
          <v-autocomplete
            :rules="nameRules"
            hint="المدينة / الدائرة"
            :items="dairaList"
            v-model="order.city"
            outlined
            label="City"
          ></v-autocomplete>
          <v-text-field
            type="number"
            hint="رقم الهاتف"
            label="Téléphone *"
            :rules="phoneRules"
            v-model="order.num1"
            outlined
          ></v-text-field>

          <v-text-field
            type="number"
            label="Téléphone 2"
            v-model="order.num2"
            hint="deuxième Téléphone/ رقم الهاتف الثاني, إذا لم نتمكن من الوصول إليك باستخدام رقم هاتفك الأول"
            outlined
          ></v-text-field>
          <v-btn dark color="#f25021" x-large @click="checkout()">
            <div class="px-7">
              <svg
                style="width: 24px; height: 24px"
                class="my-n1"
                viewBox="0 0 24 24"
              >
                <path
                  fill="currentColor"
                  d="M19 6H17C17 3.2 14.8 1 12 1S7 3.2 7 6H5C3.9 6 3 6.9 3 8V20C3 21.1 3.9 22 5 22H19C20.1 22 21 21.1 21 20V8C21 6.9 20.1 6 19 6M12 3C13.7 3 15 4.3 15 6H9C9 4.3 10.3 3 12 3M19 20H5V8H19V20M12 12C10.3 12 9 10.7 9 9H7C7 11.8 9.2 14 12 14S17 11.8 17 9H15C15 10.7 13.7 12 12 12Z"
                />
              </svg>
              Commander
            </div>
          </v-btn>
        </v-form>
      </v-col>

      <v-col cols="12" sm="4" order="first" order-sm="last">
        <v-card outlined class="pa-1">
          <v-row class="my-2" no-gutters>
            <v-col> <v-spacer></v-spacer> </v-col>
            <h4 class="font-weight-light">Order Summery</h4>
            <v-col> <v-spacer></v-spacer> </v-col>
          </v-row>
          <v-row class="px-2">
            <v-col>
              <v-list-item two-line>
                <v-list-item-avatar tile left>
                  <v-img lazy-src="logo.png" :src="product.imageUrl"></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title>{{ product.title }} </v-list-item-title>
                  <v-list-item-subtitle>
                    <h4>Qty : {{ qty }}</h4>
                    <v-btn
                      x-small
                      depressed
                      tile
                      dark
                      class="mr-1 mt-1 btn-font"
                      @click="incrementQty"
                      >+</v-btn
                    >
                    <v-btn
                      x-small
                      depressed
                      tile
                      dark
                      class="mr-1 mt-1 btn-font"
                      @click="decrementQty"
                      >-</v-btn
                    >
                  </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-col>
          </v-row>
          <v-row class="px-6">
            <v-divider></v-divider>
          </v-row>
          <v-list-item dense>
            <v-list-item-content><h3>Subtotal</h3></v-list-item-content>
            <v-list-item-action-text
              ><h3 class="text--primary">
                {{ subTotal }}
              </h3></v-list-item-action-text
            >
          </v-list-item>
          <v-list-item dense>
            <v-list-item-content
              ><h4 class="font-weight-regular">
                Frais de la livraison
                {{ `${order.wilaya ? `Pour ${order.wilaya}` : ''}` }}
              </h4></v-list-item-content
            >
            <v-list-item-action-text
              ><h3 class="font-weight-regular text--primary">
                {{ cost }}
              </h3></v-list-item-action-text
            >
          </v-list-item>
          <v-list-item dense>
            <v-list-item-content><h3>Total</h3></v-list-item-content>
            <v-list-item-action-text
              ><h3 class="font-weight-bold text--primary">
                {{ total }}
              </h3></v-list-item-action-text
            >
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog v-model="dialog" max-width="70%">
      <v-card height="50%" class="py-12">
        <v-card-text v-if="!hasError">
          <v-container style="text-align: center" class=""
            ><div class="circle-loader" :class="{ 'load-complete': done }">
              <div
                class="checkmark draw"
                :style="{ display: done ? 'block' : 'none' }"
              ></div>
            </div>
            <h2 class="main-black">
              {{ !loading ? 'تم الموافقة على طلبكم' : 'Traitement...' }}
            </h2>
            <h5 class="secondary-black">
              {{ !loading ? 'سيتم الاتصال بكم قريبا' : '' }}
            </h5>
          </v-container> </v-card-text
        ><v-card-text v-else>
          <v-container style="text-align: center" class="">
            <cross />
            <h2 class="main-black">
              {{ done ? 'Please try again!' : 'يرجى التحقق من المعلومات' }}
            </h2>
            <h5 class="secondary-black mb-12">{{ errorMessage }}</h5>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn v-if="!loading" large color="primary" @click="dialog = false"
            >Close</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-row>
      <v-col cols="12" sm="4">
        <v-card outlined tile>
          <v-list-item three-line>
            <v-list-item-avatar>
              <svg
                class="icon-color"
                style="width: 30px; height: 30px"
                viewBox="0 0 24 24"
              >
                <path
                  fill="currentColor"
                  d="M20,15.5C18.8,15.5 17.5,15.3 16.4,14.9C16.3,14.9 16.2,14.9 16.1,14.9C15.8,14.9 15.6,15 15.4,15.2L13.2,17.4C10.4,15.9 8,13.6 6.6,10.8L8.8,8.6C9.1,8.3 9.2,7.9 9,7.6C8.7,6.5 8.5,5.2 8.5,4C8.5,3.5 8,3 7.5,3H4C3.5,3 3,3.5 3,4C3,13.4 10.6,21 20,21C20.5,21 21,20.5 21,20V16.5C21,16 20.5,15.5 20,15.5M5,5H6.5C6.6,5.9 6.8,6.8 7,7.6L5.8,8.8C5.4,7.6 5.1,6.3 5,5M19,19C17.7,18.9 16.4,18.6 15.2,18.2L16.4,17C17.2,17.2 18.1,17.4 19,17.4V19Z"
                />
              </svg>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title><h4>Service client</h4></v-list-item-title>
              <v-list-item-subtitle>
                cliquer <a class="icon-color" href="tel:0552799133">ici</a> pour
                discuter avec notre équipe d'assistance
              </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
      <v-col cols="12" sm="4">
        <v-card outlined tile>
          <v-list-item three-line>
            <v-list-item-avatar>
              <svg
                class="icon-color"
                style="width: 30px; height: 30px"
                viewBox="0 0 24 24"
              >
                <path
                  fill="currentColor"
                  d="M18 18.5C18.83 18.5 19.5 17.83 19.5 17C19.5 16.17 18.83 15.5 18 15.5C17.17 15.5 16.5 16.17 16.5 17C16.5 17.83 17.17 18.5 18 18.5M19.5 9.5H17V12H21.46L19.5 9.5M6 18.5C6.83 18.5 7.5 17.83 7.5 17C7.5 16.17 6.83 15.5 6 15.5C5.17 15.5 4.5 16.17 4.5 17C4.5 17.83 5.17 18.5 6 18.5M20 8L23 12V17H21C21 18.66 19.66 20 18 20C16.34 20 15 18.66 15 17H9C9 18.66 7.66 20 6 20C4.34 20 3 18.66 3 17H1V6C1 4.89 1.89 4 3 4H17V8H20M3 6V15H3.76C4.31 14.39 5.11 14 6 14C6.89 14 7.69 14.39 8.24 15H15V6H3M10 7L13.5 10.5L10 14V11.5H5V9.5H10V7Z"
                />
              </svg>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title
                ><h4>Livraison a Domicile</h4></v-list-item-title
              >
              <v-list-item-subtitle>
                Nous offrons un service de livraison qui arrive chez vous
              </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
      <v-col cols="12" sm="4">
        <v-card outlined tile>
          <v-list-item three-line>
            <v-list-item-avatar>
              <svg style="width: 30px; height: 30px" viewBox="0 0 24 24">
                <path
                  fill="currentColor"
                  d="M2,10.96C1.5,10.68 1.35,10.07 1.63,9.59L3.13,7C3.24,6.8 3.41,6.66 3.6,6.58L11.43,2.18C11.59,2.06 11.79,2 12,2C12.21,2 12.41,2.06 12.57,2.18L20.47,6.62C20.66,6.72 20.82,6.88 20.91,7.08L22.36,9.6C22.64,10.08 22.47,10.69 22,10.96L21,11.54V16.5C21,16.88 20.79,17.21 20.47,17.38L12.57,21.82C12.41,21.94 12.21,22 12,22C11.79,22 11.59,21.94 11.43,21.82L3.53,17.38C3.21,17.21 3,16.88 3,16.5V10.96C2.7,11.13 2.32,11.14 2,10.96M12,4.15V4.15L12,10.85V10.85L17.96,7.5L12,4.15M5,15.91L11,19.29V12.58L5,9.21V15.91M19,15.91V12.69L14,15.59C13.67,15.77 13.3,15.76 13,15.6V19.29L19,15.91M13.85,13.36L20.13,9.73L19.55,8.72L13.27,12.35L13.85,13.36Z"
                />
              </svg>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title><h4>Garantie</h4></v-list-item-title>
              <v-list-item-subtitle>
                Nous vous garantissons fiabilité, rapidité et sécurité avec le
                meilleur rapport qualité-prix du marché.
              </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import cross from './Cross.vue';
export default {
  components: { cross },
  name: 'HelloWorld',
  computed: {
    subTotal() {
      return this.qty * this.product.price;
    },
    total() {
      return this.subTotal + this.cost;
    },
    wilayaList() {
      return this.wilayas
        .map((w) => {
          return { text: w.id + ' - ' + w.name, value: w.id };
        })
        .sort((a, b) => a.id - b.id);
    },
    dairaList() {
      return this.dairas.map((d) => {
        return { text: d.name, value: d.name };
      });
    },
  },
  watch: {
    'order.wilayaCode'(to) {
      this.dairas = this.wilayas.find((w) => w.id === to).dairas;
      this.order.wilaya = this.wilayas.find((w) => w.id === to).name;
      switch (to) {
        case 16:
          this.cost = 400;
          break;
        case 9:
        case 35:
        case 42:
          this.cost = 600;
          break;

        default:
          this.cost = 800;
          break;
      }
    },
  },
  async mounted() {
    await this.getProduct();
  },
  data: () => ({
    cost: 400,
    product: {
      title: '',
      price: 0,
      imageUrl: '/logo.png',
    },
    order: {
      adress: '',
      wilaya: '',
      wilayaCode: '',
      lastName: '',
      firstName: '',
      num1: '',
      num2: '',
      city: '',
    },
    url: 'http://localhost:3030',
    productId: undefined,
    qty: 1,
    wilayas: [],
    loading: false,
    dairas: [],
    dialog: false,
    done: false,
    valid: true,
    hasError: false,
    errorMessage: '',
    nameRules: [(v) => !!v || '*Required'],
    phoneRules: [
      (v) => !!v || '*Required',
      (v) =>
        v.startsWith('05') ||
        v.startsWith('06') ||
        v.startsWith('07') ||
        '*Phone Invalid',
      (v) => String(v).length == 10 || '*Phone Invalid',
    ],
  }),
  methods: {
    async getProduct() {
      this.productId = this.findGetParameter('id');

      await fetch('/result.json')
        .then((res) => res.json())
        .then(({ wilayas }) => {
          this.wilayas = wilayas;
        });

      const res = await fetch(
        this.url + '/products?productId=' + this.productId
      );
      const product = await res.json();
      this.product = product.data[0];
    },
    findGetParameter(parameterName) {
      var result = null,
        tmp = [];
      location.search
        .substr(1)
        .split('&')
        .forEach(function (item) {
          tmp = item.split('=');
          if (tmp[0] === parameterName) result = decodeURIComponent(tmp[1]);
        });
      return result;
    },
    incrementQty() {
      if (this.qty + 1 === 6) {
        return;
      }
      this.qty++;
    },
    decrementQty() {
      if (this.qty - 1 === 0) {
        return;
      }
      this.qty--;
    },

    validate() {
      this.$refs.form.validate();
    },
    async checkout() {
      //TODO: model not working
      this.hasError = false;
      if (this.done) {
        this.hasError = true;
        this.dialog = true;
        return;
      }
      this.validate();
      const children = this.$refs.form.$children;
      // if (this.isSent) {
      for (let i = 0; i < children.length; i++) {
        const child = children[i];
        if (child.errorBucket && child.errorBucket.length > 0) {
          this.errorMessage = child.label + ' - ' + child.hint;
          this.hasError = true;

          this.dialog = true;
          return;
        }
      }
      this.dialog = true;
      this.loading = true;
      await this.getProduct();

      const data = {
        adress: this.order.adress,
        city: this.order.city,
        wilaya: this.order.wilaya,
        wilayaCode: this.order.wilayaCode,
        site: true,
        lastName: this.order.lastName,
        firstName: this.order.firstName,
        num1: this.order.num1,
        num2: this.order.num2,
        products: [
          {
            productId: this.product._id,
            quantity: this.qty,
          },
        ],
      };

      const options = {
        method: 'POST',
        body: JSON.stringify(data),
        headers: {
          'Content-Type': 'application/json',
        },
      };

      const res = await fetch(this.url + '/orders', options);
      if (res.status === 201) {
        this.done = true;
        /*eslint no-undef: 0*/
        window.fbq('track', 'Purchase', {
          currency: 'DZD',
          value: res.total,
        });
      }

      this.loading = false;

      // this.$refs.form.$children.forEach((child) => {
      //   if (child.errorBucket && child.errorBucket.length > 0) {
      //     this.errorMessage = child.label + ' - ' + child.hint;
      //     this.hasError = true;
      //     return;
      //   }
      // });
      // }
    },
  },
};
</script>
<style  scoped>
.btn-font {
  font-size: 1em !important;
}

.icon-color {
  color: #242424;
}

.main-black {
  position: relative;
  max-width: 100%;
  margin: 0 0 0.4em;
  padding: 10px;
  color: #595959;
  font-size: 1.2rem;
  font-weight: 600;
  text-align: center;
  text-transform: none;
  word-wrap: break-word;
}
.secondary-black {
  color: #545454;
  font-size: 1.125rem;
}
</style>