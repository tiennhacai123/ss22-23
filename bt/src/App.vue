<template>
<div>
  <h2>Nop vao B22</h2>
    <button @click="openModal" class="open-modal-button">Add Product</button>

    <!-- Modal -->
    <div v-if="isModalOpen" class="modal">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <h2>Add Product</h2>

        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <label for="name">Name</label>
            <input type="text" v-model="form.name" id="name" required />
          </div>

          <div class="form-group">
            <label for="image">Image URL</label>
            <input type="text" v-model="form.image" id="image" required />
          </div>

          <div class="form-group">
            <label for="price">Price</label>
            <input type="number" v-model="form.price" id="price" required />
          </div>

          <div class="form-group">
            <label for="quantity">Quantity</label>
            <input type="number" v-model="form.quantity" id="quantity" required />
          </div>

          <button type="submit" class="submit-button">Add Product</button>
        </form>
      </div>
    </div>


     <div class="table">
    <div class="table-row">
      <div class="table-cell">STT</div>
      <div class="table-cell">Name</div>
      <div class="table-cell">Image</div>
      <div class="table-cell">Price</div>
      <div class="table-cell">Quantity</div>
      <div class="table-cell">Options</div>


    </div>
    <div class="table-row" v-for="(product, index) in products" :key="product.id">
      <div class="table-cell">{{ index + 1 }}</div>
      <div class="table-cell">{{ product.name }}</div>
      <div class="table-cell">
        <img :src= "product.image" alt="">
        </div>
      <div class="table-cell">{{ product.price }}$</div>
      <div class="table-cell">{{ product.quantity}}$</div>
      <div class="table-cell">
        <button @click="getProductById(product)">Get detail</button>
        <button @click="removeProductById(product.id)">Delete</button>
      </div>

    </div>
  </div>
</div>
   
</template>

<script setup>
import {ref, onMounted} from 'vue'
    const products = ref([])
    const getData = async () =>{
        const res = await fetch("http://localhost:3000/products")
        const data = await res.json();
        // console.log(data);
        products.value= data
        
    }
onMounted(() => {
    getData();
})


const getProductById = (item) => {
    console.log("Chi tiet san pham: ", "Name:", item.name, "---", "Price" , item.price, "---", "Quantity: ", item.quantity);
    
}

const removeProductById = async (id) =>{
    await fetch(`http://localhost:3000/products/${id}`,{
      method: "DELETE",
    });
    getData();
}

const isModalOpen = ref(false);
const form = ref({
  id: Math.ceil(Math.random()*1000),
  name: '',
  image: '',
  price: null,
  quantity: null
});

const openModal = () => {
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleSubmit = async () => {
  // console.log('Product Data:', form.value);
  const newProduct = form.value
  await fetch("http://localhost:3000/products",{
    method: "POST",
    headers:
    {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(newProduct)
  })
  form.value = ''
  closeModal();
  getData();
};
</script>

<style scoped>
.table {
  display: table;
  width: 700px;
  border-collapse: collapse;
  margin-top: 20px;
  font-family: Arial, sans-serif;
}

.table-row {
  display: table-row;
  background-color: #f9f9f9;
}

.table-row:nth-child(odd) {
  background-color: #f1f1f1;
}

.table-cell {
  display: table-cell;
  padding: 5px;
  border: 1px solid #ccc;
  text-align: center;
  vertical-align: middle;
}

.table-cell img {
  max-width: 150px;
  height: auto;
  border-radius: 8px;
}

.table-row:first-child {
  background-color: #4CAF50;
  color: white;
  font-weight: bold;
  text-transform: uppercase;
}

.table-cell:first-child {
  font-weight: bold;
}

.table-cell:nth-child(3) {
  width: 100px;
}


.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  width: 400px;
  position: relative;
}

.close {
  position: absolute;
  top: 10px;
  right: 15px;
  font-size: 24px;
  cursor: pointer;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.submit-button {
  background-color: #4CAF50;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.submit-button:hover {
  background-color: #45a049;
}

.open-modal-button {
  background-color: #008CBA;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.open-modal-button:hover {
  background-color: #007bb5;
}
</style>