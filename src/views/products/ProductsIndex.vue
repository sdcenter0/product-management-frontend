<template>
  <div class="page-header d-print-none">
    <div class="container-xl">
      <div class="row g-2 align-items-center">
        <div class="col">
          <!-- Page pre-title -->
          <h2 class="page-title">
            Products
          </h2>
        </div>

        <!-- Page title actions -->
        <div class="col-auto ms-auto d-print-none">
          <div class="btn-list">
            <AButton variant="primary"
                     :to="{ name: 'products.create' }">
              <IconPlus class="icon" />
              Create product
            </AButton>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="page-body">
    <div class="container-xl">

      <ATable title="Products"
              @update:sort="handleSort"
              v-model:perPage="pagination.perPage"
              v-model:page="pagination.page"
              v-model:search="search"
              :rows="data"
              :sort="sort"
              :columns="columns"
              :pagination="pagination"
              :loading="loading">
        <template #top-row>
          <td v-for="col in columns"
              :key="`filter-row-${col.name}`"
              class="p-1">
            <input type="text"
                   class="form-control form-control-sm"
                   v-model.lazy="filters[col.field]"
                   v-if="col.filterable" />
          </td>
        </template>

        <template #body-cell-id="{ value }">
          <span class="text-secondary">
            {{ value }}
          </span>
        </template>

        <template #body-cell-actions="{ row }">
          <div class="d-flex justify-content-end gap-2">
            <AButton variant="primary"
                     :to="{ name: 'products.edit', params: { id: row.id } }">
              <IconEdit class="icon" />
              Edit
            </AButton>

            <AButton variant="danger"
                     @click="handleDelete(row)"
                     :loading="deleting[row.id]">
              <IconTrash class="icon" />
              Delete
            </AButton>
          </div>
        </template>
      </ATable>
    </div>
  </div>
</template>

<script setup>
import AButton from '@/components/button/AButton.vue';
import ATable from '@/components/table/ATable.vue';
import { IconPlus } from '@tabler/icons-vue';
import { api } from '@/boot/api';
import { ref } from 'vue';
import { IconEdit } from '@tabler/icons-vue';
import { IconTrash } from '@tabler/icons-vue';
import { useResource } from '../../composables/useResource';

const columns = [
  { name: 'id', label: 'ID', field: 'id', sortable: true, filterable: true },
  { name: 'title', label: 'Title', field: 'title', sortable: true, filterable: true },
  { name: 'description', label: 'Description', field: 'description', sortable: true, filterable: true, style: { 'max-width': '200px' } },
  { name: 'price', label: 'Price', field: 'price', sortable: true, filterable: true },
  { name: 'type', label: 'Type', field: 'type', sortable: true, filterable: true },
  { name: 'actions', label: 'Actions', field: 'actions' },
];

const deleting = ref({});

const {
  fetch,
  sort,
  data,
  pagination,
  search,
  filters,
  loading
} = useResource('products');

const handleDelete = (row) => {
  if (!confirm('Are you sure you want to delete this product?')) return;

  deleting.value[row.id] = true;

  api.delete(`/products/${row.id}`)
    .then(() => fetch())
    .catch(() => alert('Something went wrong!'))
    .finally(() => {
      deleting.value[row.id] = false;
    });
}

const handleSort = (newSort) => {
  sort.value = newSort;
}
</script>