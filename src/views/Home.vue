<template>
  <v-container>
    <v-row>
      <h1>Welcome to HungryGhost Records</h1>
    </v-row>
    <v-row>
      <v-col
        v-for="product in products"
        :key="product.id"
        cols="12"
        md="6"
        lg="4"
      >
        <v-card>
          <v-img
            class="white--text align-end"
            height="200px"
            src="https://i1.sndcdn.com/artworks-000603520207-y9sj4i-t500x500.jpg"
          >
          </v-img>
          <v-card-title class="headline">{{ product.artistname }}</v-card-title>
          <v-card-subtitle>
            <v-container>
              <p class="title">{{ product.albumname }}</p>
              <v-row>
                <p class="title">${{ product.price }}</p>
                <v-spacer />
                <div>
                  <p v-if="product.quantity > 0" class="success--text">
                    {{ product.quantity }} left
                  </p>
                  <p v-else class="error--text">Out of Stock</p>
                </div>
              </v-row>
            </v-container>
          </v-card-subtitle>
          <v-card-text class="mt-n6">{{ product.description }}</v-card-text>
          <v-spacer></v-spacer>
          <v-card-actions>
            
            <v-spacer></v-spacer>

            <v-btn icon @click="show = !show">
              <v-icon>{{
                show ? "mdi-chevron-up" : "mdi-chevron-down"
              }}</v-icon>
            </v-btn>
          </v-card-actions>

          <v-expand-transition>
            <div v-show="show">
              <v-divider></v-divider>

              <v-card-text>
                <v-row align="right">
                  <v-subheader>TRACKLIST </v-subheader>
                <v-list-item>01. FEEL SPECIAL 02. RAINBOW 03. GET LOUD 04. TRICK IT
                05. LOVE FOOLISH 06. 21:29 07. BREAKTHROUGH (Korean Ver.)</v-list-item>
                </v-row>
              </v-card-text>
            </div>
          </v-expand-transition>
          <v-card-actions>
            <v-btn
              v-if="user"
              block
              color="primary"
              text
              :disabled="product.quantity == 0"
              @click="addToCart(product)"
              >Add to Cart</v-btn
            >
            <v-btn
              v-else
              block
              color="primary"
              text
              :disabled="product.quantity == 0"
              to="/login"
              >Please Login to Buy</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapGetters } from "vuex";
import { db } from "../plugins/firebase";

export default {
  name: "Home",
  data() {
    return {
      products: [],
      show: false
    };
  },
  computed: {
    ...mapGetters({
      user: "getUser"
    })
  },
  mounted() {
    this.bind();
  },
  methods: {
    async bind() {
      await this.$bind(
        "products",
        db.collection("products").where("showCatalog", "==", true)
      );
    },
    async addToCart(product) {
      await db
        .collection("cart")
        .doc(this.user.uid)
        .update({
          items: this.$firebase.firestore.FieldValue.arrayUnion({
            id: product.id,
            name: product.name,
            quantity: 1,
            price: product.price
          })
        });
    }
  }
};
</script>
