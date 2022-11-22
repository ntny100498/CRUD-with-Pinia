<template>
  <Toolbar>
    <template #start>
      <Button label="New" icon="pi pi-plus" class="p-button-success mr-2" @click="openNew" />
    </template>
  </Toolbar>
</template>

<script>
import { storeToRefs } from 'pinia';
import { useProductsStore } from '@/stores/products';

export default {
  setup() {
    const store = useProductsStore();
    const { product, submitted, productDialog } = storeToRefs(store);
    return {
      product,
      submitted,
      productDialog
    }
  },
  methods: {
    openNew() {
      this.product = {};
      this.submitted = false;
      this.productDialog = true;
    }
  }
}
</script>