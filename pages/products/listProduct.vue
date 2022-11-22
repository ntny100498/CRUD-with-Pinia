<template>
  <DataTable :value="products" responsiveLayout="scroll">
    <template #header>
      <div class="table-header flex flex-column md:flex-row md:justiify-content-between">
        <h3 class="mb-2 md:m-0 p-as-md-center">Manage Products</h3>
      </div>
    </template>
    <Column field="id" header="ID"></Column>
    <Column field="name" header="Name"></Column>
    <Column field="price" header="Price"></Column>
    <Column field="description" header="Description"></Column>
    <Column :exportable="false" style="min-width:8rem">
      <template #body="slotProps">
        <Button icon="pi pi-pencil" class="p-button-rounded p-button-success mr-2" @click="editProduct(slotProps.data)" />
        <Button icon="pi pi-trash" class="p-button-rounded p-button-warning" @click="confirmDeleteProduct(slotProps.data)" />
      </template>
    </Column>
    <productDetails/>
    <deleteProductDialog/>
  </DataTable>
</template>

<script>
import { storeToRefs } from 'pinia';
import { useProductsStore } from '@/stores/products';
import productDetails from '@/pages/products/productDetails';
import deleteProductDialog from '@/pages/products/deleteProductDialog';

export default {
  setup() {
    const store = useProductsStore();
    const { products, product, productDialog, deleteProductDialog } = storeToRefs(store);
    return {
      products,
      product,
      productDialog,
      deleteProductDialog
    }
  },
  components: {
    productDetails,
    deleteProductDialog
  },
  methods: {
    confirmDeleteProduct(data){
      this.product = data;
      this.deleteProductDialog = true;
    },
    editProduct(data) {
      this.product = data;
      this.productDialog = true;
    }
  }
}
</script>