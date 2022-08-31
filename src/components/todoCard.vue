<template>

        
    <div class="todo__card">
        <div class="card">
            <div class="card__content" v-for="card in cards" :key='card.id'>
                <div class="card__content__name">
                    <h2>{{card.cardName}}</h2>
                </div>
                <div class="card__content__text">
                    <ul>
                        <li v-for="task in card.task" :key="task.id" :class="{ready: task.isCheck}">
                            {{task.value}}
                            </li>                        
                    </ul>
                </div>
                <div class="card__content__buttons">
                    <a class="button touch delete"></a>
                    <button class="edit">Редактировать</button>
                    <button class="delete" @click="openModal(card.id)">Удалить</button>
                </div>
            </div>
        </div>
    </div>
    <Teleport to="body">
        <modal :show="showModal" @deleteCard="$emit('deleteCard', indexOfCard); showModal = false" @close='showModal = false'>
           
            <template #header>
                <h3>Удаление</h3>
                <hr/>
            </template>
            <template #body>
                <h4>Вы уверены, что хотите удалить карточку?</h4>
                <hr/>
            </template>
        </modal>
    </Teleport>

</template>

<script>
import Modal from '@/components/Modal.vue'
export default {
props: ["cards"],
emits:['load', 'deleteCard'],
data(){
    return{
        showModal: false,
        indexOfCard: null
    }
   
},
 components:{
        Modal
    },
    methods:{
        openModal(idx){
            console.log(idx)
            this.indexOfCard = idx
            this.showModal = true
        }
    }
}
</script>

<style scoped>
.ready{
    color: green;
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
}
@media screen and (max-width: 375px){
    .todo__card{
    padding: 0;
    }
}


</style>