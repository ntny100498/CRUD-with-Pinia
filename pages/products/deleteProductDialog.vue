<template>
  <Dialog v-model:visible="deleteProductDialog" :style="{ width: '450px' }" header="Confirm" :modal="true">
    <div class="confirmation-content">
      <i class="pi pi-exclamation-triangle mr-3" style="font-size: 2rem" />
      <span v-if="product">Are you sure you want to delete <b>{{ product.name }}</b>?</span>
    </div>
    <template #footer>
      <Button label="No" icon="pi pi-times" class="p-button-text" @click="deleteProductDialog = false" />
      <Button label="Yes" icon="pi pi-check" class="p-button-text" @click="deleteProduct" />
    </template>
  </Dialog>
</template>

<script>
import { storeToRefs } from 'pinia';
import { useProductsStore } from '@/stores/products';

export default {
  setup() {
    const store = useProductsStore();
    const { loading, product, deleteProductDialog } = storeToRefs(store);
    const { deleteProduct, fetchData } = store;
    return {
      loading,
      product,
      deleteProductDialog,
      deleteProduct,
      fetchData
    }
  },
  methods: {
    deleteProduct() {
      this.loading = true;
      this.deleteProduct(this.product.id)
      .then(() => {
        this.loading = false;
        this.fetchData();
        this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Deleted', life: 3000});
      })
      .catch(() => {
        this.loading = false;
        this.$toast.add({severity:'error', summary: 'Error', detail: 'Delete product error', life: 3000});
      });
      this.deleteProductDialog = false;
      this.product = {};
    }
  }
}
</script>