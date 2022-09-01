<template>
<nav-bar/>
    <h1>Создание задачи</h1>

        <!-- CARD CREATING -->

        <div class="todo__card">
        <div class="card">
            <div class="card__content">
                <div class="card__content__name">
                    <h2 v-show="!isEditName" @click="isEditName = !isEditName">{{cardName}}</h2>
                    <input class="inputName" id="inputName" type="text" v-show="isEditName" v-on:keyup.enter="submitName" v-model="cardName">
                </div>
                <div class="card__content__text">
                    <ul>

                        <div 
                        class="card__content__checkbox" 
                        v-for="(input, i) in inputs" 
                        :key="input.inpid"  
                        style="margin-top:7px;">

                          <input 
                          type="checkbox" 
                          name="checkbox" 
                          :id="input.checkid" 
                          @change="input.isCheck = !input.isCheck"> 

                          <li>
                            <input 
                                class="taskInput"
                                :class="{ready: input.isCheck}"
                                :id="input.inpid" 
                                type="text" 
                                 v-model="input.value" 
                                v-on:keyup.enter="unfocus(i)"
                                size="30"> 
                          </li>   

                          <button class="delete__task" @click="deleteInput(input.inpid)">Удалить</button>

                        </div>                       
                    </ul>
                    
                    <button class="add__task" @click="addInput">Добавить задачу</button>
                </div>
                <div class="card__content__buttons">
                    <a class="button touch delete"></a>
                    <button class="edit" @click="saveCard">Сохранить</button>
                    <button class="cancel" @click="showModal = true">Отменить</button>
                    
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL -->
    <Teleport to="body">
    <modal :show="showModal" @deleteCard="cancelCreate" @close='showModal = false'>
      <template #header>
        <h3>Отменить изменения</h3>
        <hr/>
      </template>
      <template #body>
        <h4>Вы уверены, что хотите отменить все внесенные измнения?</h4>
        <hr/>
      </template>
    </modal>
  </Teleport>

</template>

<script>
import NavBar from '@/components/NavBar.vue';
import router from '@/router';
import Modal from '@/components/Modal.vue'
export default {
  data(){
    return {
      isEditName: false,
      showModal: false,
      cardName: 'Нажмите, чтобы ввести название',      
      counter: 0,
      counterChk: 0,
      inputs: 
        [{
            inpid: 'li0',
            checkid: 'chk0',
            value: 'Введите название',   
            isCheck: false,            
        }],
    }
  },
 components: {
    NavBar,
    Modal
  },
  methods:{
    cancelCreate(){
        // cancel all changesh, just push to home.
        router.push({name: 'home'})
    },
    submitName(){
        // Changing name of card
    let inputName = document.getElementById('inputName')
    this.cardName = inputName.value
    this.isEditName = false
    },
  addInput() {
         // Creates unique id`s for input and checkbox

        this.inputs.push({
        inpid: `li${++this.counter}`,  
        checkid: `chk${++this.counterChk}`,
        value: 'Введите название',
        isCheck: false,
        });
    },
    deleteInput(id){
        // deleting item from array

        this.inputs = this.inputs.filter((input) => input.inpid !== id)
    },

    unfocus(c){
        // unfocus input when enter pressed
        let text = document.getElementById(`li${c}`)     
        text.blur()
    },
    async saveCard(){
            const response = await fetch(
                "https://todo-mkion-default-rtdb.europe-west1.firebasedatabase.app/todoCards.json",
        {
          method: "POST",
          header: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            cardName: this.cardName,
            task: this.inputs,
            
          }),
        }
      );
      router.push({name: 'home'})
    },
    }
}
</script>

<style>
.ready{
    color: green;
}
h1 {
    color: #000;
    text-align: center;
}
.todo__card{
    padding: 70px 550px 70px 550px;
}
.card {
    border-radius: 7px;
    padding:10px 20px  20px 20px;
}

.card__content {
     background: #ffff;
    box-shadow: 0 0 10px 5px rgba(221, 221, 221, 1);
    margin-top: 10px;
    border-radius: 5px;
    text-align: left;
}
.card__content__name{
    padding: 10px 0 10px 0;
}
.card__content__text{
    padding: 10px 0 10px 0;
}
span, h2{
    padding: 10px 10px 0 20px;
    margin-top: 0 !important;
    margin-bottom: 0 !important;
}
.card__content__buttons{
    display: flex;
    justify-content: flex-end;
    padding-bottom: 20px;
    padding-right: 20px;
}
.edit{
    margin-right: 7px;
    border: 0;
    background: green;
    color: #fff;
    cursor: pointer;
    border-radius: 3px;
    padding: 5px;
}
.delete{
   border: 0;
    background: red;
    color: #fff;
    cursor: pointer;
    border-radius: 3px;
    
}
.delete__task{
   border: 0;
    background: red;
    color: #fff;
    cursor: pointer;
    border-radius: 3px;
    margin-left: 7px;
}
.cancel{
   border: 0;
    background: blue;
    color: #fff;
    cursor: pointer;
    border-radius: 3px;
    margin-right: 7px;
    
}
.add__task{
   border: 0;
    background: green;
    color: #fff;
    cursor: pointer;
    border-radius: 3px;
    margin-left: 40px;
    padding: 5px;
}
.card__content__checkbox{
  display: flex;
}
li{
  margin-left: 20px;
}
.inputName{
  margin-top: 10px;
  margin-left: 20px;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid grey;
}
.taskInput{
    border: 0;
    font-size: 16px;
    outline: 0;
}
@media screen and (max-width: 1600px){
   .todo__card{
    padding: 70px 450px 70px 450px;
    }
}
@media screen and (max-width: 1440px){
    .todo__card{
    padding: 70px 350px 70px 350px;
    }
}
@media screen and (max-width: 1024px){
     .todo__card{
    padding: 70px 150px 70px 150px;
    }
}
@media screen and (max-width: 768px){
    .todo__card{
    padding: 40px 70px 40px 70px;
    }
}
@media screen and (max-width: 425px){
    .todo__card{
    padding: 15px;
    }
    h2{
        font-size: 1em;
    }
    li{
        font-size: 1em;
    }
    .taskInput{
      width: 150px;
    }
}
@media screen and (max-width: 375px){
    .todo__card{
    padding: 0;
    }
    .taskInput{
      width: 100px;
      font-size: 12px;
    }
    ul{
      margin-left: -25px;
    }
    .add__task{
      margin-left: 20px;
      margin-top: 20px;
    }
    .card__content__text{
      padding-bottom: 40px;
    }
    button{
      font-size: 12px;
    }
}

</style>