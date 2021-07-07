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
    <button>Save</button>
</form>
</template>

<script>
import { onMounted, ref } from 'vue'
import { useRouter, useRoute } from 'vue-router'

export default {
    name: "ProductEdit",
    props: ['id'],
    setup() {
       const title = ref(''); 
       const image = ref('');
       const details = ref('');
       const price = ref('');
       const router = useRouter();
       const route = useRoute()
       onMounted(async() => {
           const res = await fetch(`http://localhost:4000/api/products/${route.params.id}`);
           const product = await res.json();
           title.value = product.title;
           image.value = product.image;
           details.value = product.details;
           price.value = product.price;
       })

       const submit = async () => {
           await fetch(`http://localhost:4000/api/products/${route.params.id}`, {
               method: 'PUT',
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
