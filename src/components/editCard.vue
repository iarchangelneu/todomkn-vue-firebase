<template>
  
    
   <nav-bar/>
    <h1>Редактирование Карточки</h1>

        <!-- CARD EDITING -->

        <div class="todo__card">
        <div class="card">
            <div class="card__content">
                <div class="card__content__name">
                    <h2 v-show="!isEditName" @click="isEditName = !isEditName">{{cardsList.cardName}}</h2>
                    <input class="inputName" id="inputName" type="text" v-show="isEditName" v-on:keyup.enter="submitName" v-model="cardsList.cardName">
                </div>
                <div class="card__content__text">
                    <ul>
                      <li 
                      v-for="card in cardsList.task" 
                      :key="card.inpid"
                      :class="{ready : card.isCheck}"
                      > <input 
                      type="checkbox"  
                      :checked='card.isCheck' 
                      @click="card.isCheck = !card.isCheck">
                        <input 
                        type="text" 
                        :id="card.inpid"
                        v-model="card.value" 
                        :class="{ready : card.isCheck}">
                        <button class="delete__task" @click="deleteInput(card.inpid)">Удалить</button>
                      </li>
                      
                        
                    </ul>
                    <button class="add__task" @click="$emit('addInput')">Добавить задачу</button>
                </div>
                <div class="card__content__buttons">
                    <a class="button touch delete"></a>
                    <button class="edit" @click="editContent">Сохранить</button>
                    <button class="cancel" @click="showModal = true; cancel = true">Отменить</button>
                    <button class="delete" @click="showModal = true; cancel = false">Удалить</button>
                    
                </div>
            </div>
        </div>
    </div>
    <!-- MODALS  -->
        <Teleport to="body">
        <modal :show="showModal" @deleteCard="deleteCard(this.cardId); showModal = false" @close='showModal = false'>
           
            <template #header>
                <h3 v-show="cancel">Отменить изменения</h3>
                <h3 v-show="!cancel">Удаление</h3>
                <hr/>
            </template>
            <template #body>
                <h4 v-show="cancel">Вы уверены, что хотите отменить все внесенные изменения?</h4>
                <h4 v-show="!cancel">Вы уверены, что хотите удалить карточку?</h4>
                <hr/>
            </template>
            <template #footer v-if="cancel">
                 <button
                class="modal-default-button no__btn"
                @click="showModal = false"
              >Нет</button>
              <button
                class="modal-default-button yes__btn"               
                @click="$emit('loadCards'); showModal = false"
              >Да</button>
            </template>
        </modal>
    </Teleport>

</template>

<script>
import NavBar from '@/components/NavBar.vue';
import router from '@/router';
import Modal from '@/components/Modal.vue'
import axios from "axios";
export default {
    props: ["cardsList", "cardId"],
emits:['loadCards', 'addInput', 'deleteInput'],
components:{
  NavBar,
  Modal
},
data(){
  return{
    isEditName: false,
    showModal: false,
    cancel: null
  }
},
methods:{
      submitName(){
        // Changing name of card
    let inputName = document.getElementById('inputName')
    this.cardName = inputName.value
    this.isEditName = false
    },
    async editContent() {
      const response = await fetch(
        'https://todo-mkion-default-rtdb.europe-west1.firebasedatabase.app/todoCards/' + this.cardId + '.json',
        {
          method: "PUT",
          header: {
            "Content-Type": "application/json; charset=UTF-8'",
          },
          body: JSON.stringify({
            cardName: this.cardsList.cardName,
            task: this.cardsList.task
          }),
        }
      );
      router.push({name: 'home'})
    },
     deleteInput(id){
          this.cardsList.task = this.cardsList.task.filter((card) => card.inpid !== id)
    },
       deleteCard(id){
        axios.delete(`https://todo-mkion-default-rtdb.europe-west1.firebasedatabase.app/todoCards/${id}.json`);
        setTimeout(() => {
        router.push({name: 'home'});
      }, 100);
    },
                    
    },
}
</script>

<style>
input{
  border: 0;
  font-size:16px;
}
</style>