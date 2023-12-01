<script lang="ts" setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
const props = defineProps({
  product: {
    type: Object,
    default: {},
  },
});

const { baseStorageUrl } = useAppConfig();

const isDeleting = ref(false);
const router = useRouter();

const deleteProduct = async () => {
  // You can implement the logic to delete the product here
  // For example, you can call an API to delete the product
  try {
    // Assuming you have an API endpoint for deleting a product
    await api.delete(`/products/${props.product.id}`);
    // Optionally, you can emit an event to inform the parent component that the product is deleted
    // emit('productDeleted', props.product.id);
    // Redirect to a different route or perform other actions as needed
    router.push('/products');
  } catch (error) {
    console.error('Error deleting product', error);
  }
};
</script>

<template>
  <section class="bg-white shadow-xl rounded-xl overflow-hidden">
    <div :class="`w-full h-[200px] p-5 bg-blue-300`">
      <img
        :src="baseStorageUrl + props.product.image"
        class="w-full h-full object-contain"
      />
    </div>
    <div class="px-5 pb-5 pt-9 relative">
      <NuxtLink :to="`/product/${props.product.id}`"
        ><h3 class="text-lg font-bold mb-4 text-limit limit-2">
          {{ props.product.name }}
        </h3></NuxtLink
      >

      <div class="flex justify-between items-center">
        <span class="text-sm font-normal">{{ props.product.category }}</span>
        <span class="text-sm font-normal">${{ props.product.price }}</span>
      </div>
      <br>
   
      <div>
        <span class="text-sm font-normal">{{ props.product.description }}</span>
      </div>
      
      <!-- Shopping Cart Button -->
      <div
        :class="`cursor-pointer absolute -top-5 right-7 w-[50px] h-[50px]
        ${props.product.isCart ? 'bg-blue-600 text-white' : 'bg-white'} shadow-xl
        rounded-full flex justify-center items-center hover:bg-blue-600 hover:text-white
        transition duration-300`"
      >
        <i class="ri-shopping-cart-2-line"></i>
      </div>

      <!-- Delete Button -->
      <div
        @click="deleteProduct"
        :class="`cursor-pointer absolute -top-5  w-[50px] h-[50px]
          bg-white text-gray-500 shadow-xl rounded-full flex justify-center items-center
          hover:bg-red-600 hover:text-white transition duration-300 ${isDeleting ? 'opacity-50' : ''}`"
        v-if="!isDeleting"
      >
        <i class="ri-delete-bin-7-fill"></i>
      </div>

      <!-- Show a loading spinner while deleting -->
      <div v-if="isDeleting" class="absolute -top-5 right-7 w-[50px] h-[50px] flex justify-center items-center">
        <i class="ri-loader-line animate-spin text-gray-500"></i>
      </div>
    </div>
  </section>
</template>
