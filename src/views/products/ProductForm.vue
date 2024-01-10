<template>
  <div class="page-body">

    <div class="container-xl">
      <form @submit.prevent="handleSubmit">
        <div class="card">
          <div class="card-header p-2">
            <div class="d-flex justify-content-between align-items-center w-100 gap-2">
              <h2 class="card-title">
                <slot name="title"></slot>
              </h2>

              <div class="d-inline-flex gap-2">
                <slot name="actions"></slot>
              </div>
            </div>
          </div>

          <div style="height: 1px;"
               :class="{ invisible: !loading }"
               class="progress rounded-0">
            <div class="progress-bar progress-bar-indeterminate"></div>
          </div>

          <div class="card-body">

            <div class="row">
              <div class="col-md-6">

                <AInput class="mb-2"
                        type="text"
                        label="Product name"
                        :invalid="'title' in validation"
                        :errors="validation.title"
                        v-model="product.title" />

                <AInput class="mb-2"
                        type="textarea"
                        label="Description"
                        :invalid="'description' in validation"
                        :errors="validation.description"
                        v-model="product.description" />

                <AInput class="mb-2"
                        type="number"
                        label="Price"
                        step=".01"
                        :invalid="'price' in validation"
                        :errors="validation.price"
                        v-model.number="product.price" />

                <AInput class="mb-2"
                        type="text"
                        label="Type"
                        :invalid="'type' in validation"
                        :errors="validation.type"
                        v-model.number="product.type" />
              </div>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import AInput from '../../components/input/AInput.vue';

const emit = defineEmits(['submit']);

defineProps({
  loading: Boolean,

  validation: {
    type: Object
  }
});

const product = defineModel();

const handleSubmit = () => {
  emit('submit', product.value);
};
</script>