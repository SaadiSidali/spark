<template>
  <v-container>
    <v-row>
      <v-col cols="12" sm="8">
        <h1 class="mb-3">Livraison</h1>
        <v-divider class="mb-12"></v-divider>
        <v-row>
          <v-col cols="12" sm="6">
            <v-text-field
              error-messages="ass"
              label="Prenom"
              outlined
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6">
            <v-text-field
              success
              success-messages="yay"
              :aria-autocomplete="false"
              label="Nom"
              outlined
            ></v-text-field>
          </v-col>
        </v-row>
        <v-text-field label="Adress" outlined></v-text-field>

        <v-autocomplete
          :loading="true"
          disabled
          v-model="value"
          :items="['alger', 'batna']"
          outlined
          label="Wilaya"
        ></v-autocomplete>
        <v-autocomplete
          v-model="value"
          :items="['Cheraga', 'Bouchaoui']"
          outlined
          label="City"
        ></v-autocomplete>
        <v-text-field label="Phone 1" outlined></v-text-field>
        <v-text-field
          label="Phone 2"
          hint="deuxième Téléphone/ رقم الهاتف الثاني, إذا لم نتمكن من الوصول إليك باستخدام رقم هاتفك الأول"
          outlined
        ></v-text-field>
        <v-btn x-large color="#F25021" dark @click="checkout()">
          <div class="pa-7">
            <v-icon class="mr-2">mdi-cart</v-icon> Commander
          </div>
        </v-btn>
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
                <v-list-item-avatar tile>
                  <v-img
                    lazy-src="https://picsum.photos/id/11/10/6"
                    aspect-ratio="1"
                    src="https://picsum.photos/id/11/500/300"
                  ></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title
                    >مكتب عجلة القيادة - Steering Wheel Desk
                  </v-list-item-title>
                  <v-list-item-subtitle>
                    <h4>Qty : 1</h4>
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
              ><h3 class="text--primary">399.00</h3></v-list-item-action-text
            >
          </v-list-item>
          <v-list-item dense>
            <v-list-item-content
              ><h4 class="font-weight-regular">Cost</h4></v-list-item-content
            >
            <v-list-item-action-text
              ><h3 class="font-weight-regular text--primary">
                100
              </h3></v-list-item-action-text
            >
          </v-list-item>
          <v-list-item dense>
            <v-list-item-content><h3>Total</h3></v-list-item-content>
            <v-list-item-action-text
              ><h3 class="font-weight-bold text--primary">
                1,099.00
              </h3></v-list-item-action-text
            >
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog v-model="dialog" max-width="70%">
      <v-card height="50%" class="py-12">
        <v-card-text>
          <v-container style="text-align: center" class=""
            ><div class="circle-loader" :class="{ 'load-complete': done }">
              <div
                class="checkmark draw"
                :style="{ display: done ? 'block' : 'none' }"
              ></div>
            </div>
            <h2 class="main-black">تم الموافقة على طلبكم</h2>
            <h5 class="secondary-black">سيتم الاتصال بكم قريبا</h5></v-container
          >
          <svg
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 130.2 130.2"
          >
            <circle
              class="path circle"
              fill="none"
              stroke="#D06079"
              stroke-width="2"
              stroke-miterlimit="10"
              cx="65.1"
              cy="65.1"
              r="62.1"
            />
            <line
              class="path line"
              fill="none"
              stroke="#D06079"
              stroke-width="3"
              stroke-linecap="round"
              stroke-miterlimit="10"
              x1="34.4"
              y1="37.9"
              x2="95.8"
              y2="92.3"
            />
            <line
              class="path line"
              fill="none"
              stroke="#D06079"
              stroke-width="3"
              stroke-linecap="round"
              stroke-miterlimit="10"
              x1="95.8"
              y1="38"
              x2="34.4"
              y2="92.2"
            />
          </svg>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({ dialog: false, done: false }),
  methods: {
    checkout() {
      this.dialog = true;
      setTimeout(() => {
        this.done = true;
      }, 1500);
    },
  },
};
</script>
<style  scoped>
svg {
  width: 100px;
  display: block;
  margin: 40px auto 0;
}
.path {
  stroke-dasharray: 1000;
  stroke-dashoffset: 0;
}
.path.circle {
  -webkit-animation: dash 0.9s ease-in-out;
  animation: dash 0.9s ease-in-out;
}
.path.line {
  stroke-dashoffset: 1000;
  -webkit-animation: dash 0.9s 0.35s ease-in-out forwards;
  animation: dash 0.9s 0.35s ease-in-out forwards;
}
.path.check {
  stroke-dashoffset: -100;
  -webkit-animation: dash-check 0.9s 0.35s ease-in-out forwards;
  animation: dash-check 0.9s 0.35s ease-in-out forwards;
}
p {
  text-align: center;
  margin: 20px 0 60px;
  font-size: 1.25em;
}
p.success {
  color: #73af55;
}
p.error {
  color: #d06079;
}
@-webkit-keyframes dash {
  0% {
    stroke-dashoffset: 1000;
  }
  100% {
    stroke-dashoffset: 0;
  }
}
@keyframes dash {
  0% {
    stroke-dashoffset: 1000;
  }
  100% {
    stroke-dashoffset: 0;
  }
}
@-webkit-keyframes dash-check {
  0% {
    stroke-dashoffset: -100;
  }
  100% {
    stroke-dashoffset: 900;
  }
}
@keyframes dash-check {
  0% {
    stroke-dashoffset: -100;
  }
  100% {
    stroke-dashoffset: 900;
  }
}

.main-black {
  position: relative;
  max-width: 100%;
  margin: 0 0 0.4em;
  padding: 0;
  color: #595959;
  font-size: 1.875em;
  font-weight: 600;
  text-align: center;
  text-transform: none;
  word-wrap: break-word;
}
.secondary-black {
  color: #545454;
  font-size: 1.125em;
}
</style>