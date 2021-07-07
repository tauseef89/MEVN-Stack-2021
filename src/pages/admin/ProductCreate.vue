<template>
  <form @submit.prevent="submit" >
    <label>Title</label>
    <input type="text" name="title" v-model="title"                
    />
    <label>Image</label>
    <input type="text" name="image" v-model="image" 
    />
    <label>Details</label>
    <input type="text" name="details" v-model="details" 
    />
    <label>Price</label>
    <input type="text" name="price" v-model="price" 
    />
    <button>Add Product</button>
</form>
</template>

<script>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

export default {
    name: "ProductCreate",
    setup() {
       const title = ref(''); 
       const image = ref('');
       const details = ref('');
       const price = ref('');
       const router = useRouter();

       const submit = async () => {
           await fetch('http://localhost:4000/api/products', {
               method: 'POST',
               headers: {"Content-type": "application/json"},
               body: JSON.stringify({ 
                   title: title.value,
                   image: image.value,
                   details: details.value,
                   price: price.value
                })
           });

           await router.push('/admin/products');
       }
       
       return {title, image, details, price, submit}
    }
    
}
</script>

<style>

</style>