<template>
  <div>
    <ul v-for="user in users" :key="user.id">
      <li>{{ user.name }}</li>
      <li>
        <button @click="deleteUser(user.id)">delete</button>
        <button @click="updateUser(user.id)">update</button>
        </li>
      
    </ul>
    <input type="text" name="" id="" v-model="inputValue" />
    <button @click="addUser">Them du lieu vao json server</button>

  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
// khai bao ham lay du lieu tu json-sever
const inputValue = ref("");

const getData = async () => {
  const res = await fetch("http://localhost:3000/user");
  const data = await res.json();
  console.log(11111, data);
  users.value = data;
};
onMounted(() => {
  getData();
});
const users = ref([]);

// ham them

const addUser = async () => {
  const newUser = {
    name: inputValue.value,
  };
  await fetch("http://localhost:3000/user", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(newUser),
  });
  getData();
    inputValue.value=''
};

//ham xoa

const  deleteUser = async (id)=>{
    console.log(id);
    await fetch(`http://localhost:3000/user/${id}`,{
        method: "DELETE",
    })
      getData();

}

//ham cap nhat
const updateUser = async (id)=>{
    const updateName = {
        name:"Thuong Huyen",
    };
    await fetch(`http://localhost:3000/user/${id}`,{
        method: "PATCH",
        headers:
        {
            "Content-Type": "application/json",
        },
        body: JSON.stringify(updateName)
    });
    getData();
}

</script>

<style></style>
