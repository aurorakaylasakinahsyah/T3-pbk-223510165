<template>
  <div class="app">
    <h1>HI, WELCOME</h1>
    <input type="text" v-model="newItem" placeholder="Telusuri To do List Saya" @keyup.enter="addItem">
    <ul>
      <li v-for="(item, index) in items" :key="index">
        <!-- Gunakan blok div untuk menampilkan output -->
        <div class="item-block">
          <div>
            <span :class="{ 'completed': item.completed }">{{ item.text }}</span>
          </div>
          <!-- Pindahkan tombol Edit, Done, dan Delete ke dalam blok -->
          <div>
            <button @click="editItem(index)">Edit</button>
            <button @click="toggleCompleted(index)">{{ item.completed ? 'Undo' : 'Done' }}</button>
            <button @click="deleteItem(index)">Delete</button>
          </div>
          <!-- Pindahkan input untuk pengeditan di bawah tombol-tombol -->
          <input v-if="item.editing" type="text" v-model="item.updatedText" @keyup.enter="saveEdit(index)" @blur="cancelEdit(index)">
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem: '',
      items: []
    };
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        this.items.push({ text: this.newItem, completed: false });
        this.newItem = '';
      }
    },
    editItem(index) {
      // Tandai item yang diedit
      this.items[index].editing = true;
      // Simpan teks asli untuk pengeditan
      this.items[index].updatedText = this.items[index].text;
    },
    saveEdit(index) {
      // Perbarui teks item dan nonaktifkan mode pengeditan
      if (this.items[index].updatedText.trim() !== '') {
        this.items[index].text = this.items[index].updatedText;
        this.items[index].editing = false;
      }
    },
    cancelEdit(index) {
      // Batal mode pengeditan dan kembalikan teks ke teks asli
      this.items[index].editing = false;
      this.items[index].updatedText = this.items[index].text;
    },
    deleteItem(index) {
      this.items.splice(index, 1);
    },
    toggleCompleted(index) {
      this.items[index].completed = !this.items[index].completed;
    }
  }
};
</script>

<style scoped>
.app {
  font-family: 'Gill Sans', 'Gill Sans MT','Trebuchet MS';
  max-width: 400px;
  margin: 0 auto;
  background: linear-gradient(135deg, #f22693c9 0%, #e71cc2e5 100%);
  padding: 50px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px 0px rgba(245, 22, 208, 0.811);
}
input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 5px;
}
button {
  margin-left: 5px;
  background-color: #f60475f1;
  border: none;
  color: white;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #fc32abd2;
}
.completed {
  text-decoration: line-through;
}
</style>
