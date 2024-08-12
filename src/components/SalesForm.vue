<template>
  <div class="container mt-5">
    <h2>Enregistrement des ventes</h2>
    <form @submit.prevent="addSale">
      <div class="form-group">
        <label for="reference">Référence :</label>
        <input
          type="text"
          v-model="sale.reference"
          id="reference"
          class="form-control"
          required
        />
        <div v-if="errors.reference" class="text-danger">{{ errors.reference }}</div>
      </div>
      <div class="form-group">
        <label for="designation">Désignation :</label>
        <input
          type="text"
          v-model="sale.designation"
          id="designation"
          class="form-control"
          required
        />
        <div v-if="errors.designation" class="text-danger">{{ errors.designation }}</div>
      </div>
      <div class="form-group">
        <label for="quantity">Quantité vendue :</label>
        <input
          type="number"
          v-model.number="sale.quantity"
          id="quantity"
          class="form-control"
          required
        />
        <div v-if="errors.quantity" class="text-danger">{{ errors.quantity }}</div>
      </div>
      <div class="form-group">
        <label for="price">Prix de vente :</label>
        <input
          type="number"
          v-model.number="sale.price"
          id="price"
          class="form-control"
          step="0.01"
          required
        />
        <div v-if="errors.price" class="text-danger">{{ errors.price }}</div>
      </div>
      <button type="submit" class="btn btn-primary">Enregistrer la vente</button>
    </form>

    <h3 class="mt-5">Ventes enregistrées</h3>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Référence</th>
          <th>Désignation</th>
          <th>Quantité</th>
          <th>Prix</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(sale, index) in sales" :key="index">
          <td>{{ sale.reference }}</td>
          <td>{{ sale.designation }}</td>
          <td>{{ sale.quantity }}</td>
          <td>{{ sale.price.toFixed(2) }} €</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const sale = ref({
  reference: '',
  designation: '',
  quantity: 0,
  price: 0,
});

const sales = ref([]);
const errors = ref({});

const addSale = () => {
  errors.value = {};

  if (!sale.value.reference) {
    errors.value.reference = 'La référence est requise.';
  }
  if (!sale.value.designation) {
    errors.value.designation = 'La désignation est requise.';
  }
  if (sale.value.quantity <= 0) {
    errors.value.quantity = 'La quantité doit être positive.';
  }
  if (sale.value.price <= 0) {
    errors.value.price = 'Le prix doit être positif.';
  }

  if (Object.keys(errors.value).length === 0) {
    sales.value.push({ ...sale.value });
    sale.value = { reference: '', designation: '', quantity: 0, price: 0 };
  }
};
</script>

<style scoped>
.text-danger {
  font-size: 0.875em;
}
.table{
  color: grey;
}
</style>
