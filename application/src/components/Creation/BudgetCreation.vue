<template>
  <div class="l-budget-creation">
    <v-layout row wrap>
      <v-flex xs12 md2>
        <v-select label="Status" :items="states" v-model="budget.state"></v-select>
      </v-flex>
      <v-flex xs12 md9 offset-md1>
        <v-select label="Client" :items="clients" v-model="budget.client_id" item-text="name" item-value="_id"></v-select>
      </v-flex>
      <v-flex xs12 md12>
        <v-text-field label="Title" v-model="budget.title" required color="light-blue lighten-1"></v-text-field>    
        <v-text-field label="Description" v-model="budget.description" textarea required color="light-blue lighten-1"></v-text-field>    
      </v-flex>
            <v-layout row wrap v-for="item in budget.items" class="l-budget-item" :key="item.id">
        <v-flex xs12 md1>
          <v-btn block dark color="red lighten-1" @click.native="removeItem(item)">Remove</v-btn>
        </v-flex>

        <v-flex xs12 md3 offset-md1>
          <v-text-field label="Title"
                        v-model="item.title"
                        required
                        color="blue">
          </v-text-field>
        </v-flex>

        <v-flex xs12 md1 offset-md1>
          <v-text-field label="Price"
                        prefix="$"
                        v-model="item.price"
                        required
                        color="blue">
          </v-text-field>
        </v-flex>

        <v-flex xs12 md2 offset-md1>
          <v-text-field label="Quantity"
                        min="0"
                        v-model="item.quantity"
                        type="number"
                        required
                        color="blue"
                        class="black--text">
          </v-text-field>
        </v-flex>

        <v-flex xs12 md2>
          <span class="md-budget-item-subtotal black--text">ITEM PRICE $ {{ item.subtotal }}</span>
        </v-flex>
      </v-layout>

      <v-flex xs12 md2 offset-md10>
        <v-btn block color="md-add-item-btn blue" @click.native="addItem()">Add item</v-btn>
      </v-flex>

      <v-flex xs12 md2 offset-md10>
        <span class="md-budget-item-total black--text">TOTAL $ {{ budget.total_price }}</span>
      </v-flex>

      <v-flex xs12 md2 offset-md10>
        <v-btn block color="md-add-item-btn green" @click.native="saveBudget(budget)">Save</v-btn>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
export default {
  props: ["clients", "saveBudget"],
  data() {
    return {
      budget: {
        title: null,
        description: null,
        state: "writing",
        client: null,
        get total_price() {
          let value = 0;
          this.items.forEach(({ subtotal }) => {
            value += parseInt(subtotal);
          });
          return value;
        },
        items: [
          {
            title: null,
            quantity: 0,
            price: 0,
            get subtotal() {
              return this.quantity * this.price;
            }
          }
        ]
      },
      states: ["writing", "editing", "pending", "approved", "denied", "waiting"]
    };
  },
  methods: {
    addItem() {
      const items = this.budget.items;
      const item = {
        title: "",
        quantiti: 0,
        price: 0,
        get subtotal() {
          if (this.quantity && this.price) return this.quantity * this.price;
          else return 0;
        }
      };

      items.push(item);
    },

    removeItem(selected) {
      const items = this.budget.items;
      items.forEach((item, index) => {
        if (item === selected) {
          items.splice(index, 1);
        }
      });
    }
  }
};
</script>
