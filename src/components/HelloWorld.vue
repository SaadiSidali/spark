<template>
  <v-container>
    <v-row>
      <v-col cols="12" sm="8">
        <h1 class="mb-3">Livraison</h1>
        <v-divider class="mb-12"></v-divider>
        <v-row>
          <v-col cols="12" sm="6">
            <v-text-field label="Prenom" outlined></v-text-field>
          </v-col>
          <v-col cols="12" sm="6">
            <v-text-field label="Nom" outlined></v-text-field>
          </v-col>
        </v-row>
        <v-text-field label="Adress" outlined></v-text-field>

        <v-autocomplete
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
        <v-text-field label="phone 1" outlined></v-text-field>
        <v-text-field label="phone 2" outlined></v-text-field>
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
    <v-dialog v-model="dialog" max-width="290">
      <v-card width="500px">
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