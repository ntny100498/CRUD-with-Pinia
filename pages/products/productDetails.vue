<template>
  <Dialog header="Product Details" v-model:visible="productDialog" :style="{width: '450px'}" :modal="true" class="p-fluid">
    <div class="field">
      <label for="name">Name</label>
      <InputText id="name" v-model.trim="product.name" required="true" autofocus
        :class="{ 'p-invalid': submitted && !product.name }" />
      <small class="p-error" v-if="submitted && !product.name">Name is required.</small>
    </div>
    <div class="field">
      <label for="description">Description</label>
      <Textarea id="description" v-model="product.description" required="true" rows="3" cols="20" />
    </div>
    <div class="field">
      <label for="price">Price</label>
      <InputNumber id="price" v-model="product.price" mode="currency" currency="VND" locale="vi-VN" />
    </div>
    <template #footer>
      <Button label="Cancel" icon="pi pi-times" class="p-button-text" @click="hideDialog"/>
      <Button label="Save" icon="pi pi-check" class="p-button-text" @click="saveProduct" />
    </template>
  </Dialog>
</template>
<script>
import { storeToRefs } from 'pinia';
import { useProductsStore } from '@/stores/products';

export default {
  setup() {
    const store = useProductsStore();
    const { product, productDialog, submitted } = storeToRefs(store);
    const { createProduct, updateProduct, fetchData } = store;
    return {
      product,
      productDialog,
      submitted,
      createProduct,
      updateProduct,
      fetchData
    }
  },
  methods: {
    hideDialog() {
      this.productDialog = false;
      this.submitted = false;
    },
    saveProduct() {
      this.submitted = true;
      this.loading = true;
      if (this.product.name && this.product.name.trim()) {
        if (this.product.id) {
          this.updateProduct(this.product)
          .then(() => {
            this.loading = false;
            this.fetchData();
            this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Updated', life: 3000});
          })
          .catch(() => {
            this.loading = false;
            this.$toast.add({severity:'error', summary: 'Error', detail: 'Update product error', life: 3000});
          });
        }
        else {
          this.createProduct(this.product)
          .then(() => {
            this.loading = false;
            this.fetchData();
            this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Created', life: 3000});
          })
          .catch(() => {
            this.loading = false;
            this.$toast.add({severity:'error', summary: 'Error', detail: 'Create product error', life: 3000});
          });
        }
        this.productDialog = false;
        this.product = {};
      }
    }
  }
}
</script>