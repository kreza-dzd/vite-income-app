<script setup>
    import {ref} from "vue";

    import HeaderView from './components/HeaderView.vue';
   

    const showModal = ref(false)
    const newNote = ref("")
    const errorMessage = ref("")
    const notes = ref([])
    const newNumb = ref("")
   


    function getRandomColor() {
      return "hsl(" + Math.random() * 360 + ", 100%, 75%)";

}


    const addNote = () => {
      if (newNote.value.length < 5) {
        return errorMessage.value = "Note needs to be 10 characters or more"
      } if (newNote.value.length > 140) {
        return errorMessage.value = "Note needs to be max 140 characters"
      }

        notes.value.push({
          id: Math.floor(Math.random() * 100000),
          text: newNote.value,
          numb: newNumb.value,
          date: new Date(),
          backgroundColor: getRandomColor()
        });

      
        showModal.value = false;
        newNote.value = ""
        newNumb.value = ""
        errorMessage.value = ""


        
      

      }


      const removeItem = note => {
          notes.value = notes.value.filter(n => n !== note)
        }


  
</script>

<template>
  <HeaderView />
 <main>
 <div v-if="showModal" class="overlay">
  <div class="modal">
    <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
    <input v-model="newNumb" type="number" class="numb">
    <p v-if="errorMessage">{{ errorMessage }}</p>
    <button @click="addNote">Add Income</button>
    <button @click="showModal = false" class="close">Close</button>
  </div>
</div> 
  <div class="container">
  <header>
    <h1>Income </h1>
    <button @click="showModal = true">+</button>
  </header> 
  <div class="cards-container">  
    <div v-for="note in notes" :style="{backgroundColor: note.backgroundColor }" :key="note.id" class="card">
      <div class="actions">
        <button @click="removeItem(note)" class="delete">x</button>
      </div>
      <p class="main-text">{{ note.text }}</p>
      <div class="paras">
        <p class="para-title">Income :</p>
        <p class="main-text2">{{ note.numb }}€</p>
      </div>
        <p class="date">{{ note.date.toLocaleString("CET") }}</p>
    </div>
  </div>
</div>
</main>
</template>

<style scoped>
  main {
     height: 100vh;
     width: 100vw;
  }

  .container {
    max-width: 700px;
    padding: 10px;
    margin: 0 auto;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font: 75px;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21, 20, 20);
    border-radius: 100%;
    color: white;
    font-size: 20px;

  }

  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .date {
    font-size: 12.5px;
    font-weight: bold;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;

    display: flex;
    flex-direction: column;
  }

  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }

  .modal .close {
    background-color: darkred;
    margin-top: 7px;
  }

  .modal p {
    color: darkred;
  }

  .paras {
    display: inline-block;
  }

  .para-title {
    float:left;
    width:50%
  }
  .main-text2 {
    float:right;
    width:50%
  }


.actions .delete {
  background: transparent;
	padding: 0.5rem;
	border: none;
	color: #FFF;
	cursor: pointer;
	transition: 0.2s ease-in-out;
  float: right;
}
</style>
