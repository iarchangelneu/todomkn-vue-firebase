<template>
<nav-bar/>
  <h1>Список задач</h1>

  <!-- CARD LIST  -->
  <todo-card
  :cards="cards"
  @load="loadCards"
  @deleteCard="deleteCard"
  ></todo-card>


<!-- MODAL -->
     
</template>

<script>
import todoCard from '@/components/todoCard.vue';
import NavBar from '@/components/NavBar.vue';
import axios from "axios";

export default {
  components: {
    todoCard,
    NavBar,
  },
  data(){
    return{
      cards:[],
    }
  },
  methods:{
    async loadCards(){
      try{
        const {data} = await axios.get(
          "https://todo-mkion-default-rtdb.europe-west1.firebasedatabase.app/todoCards.json"
          );
          if(!data){
            throw new Error('Список пуст');
          }
          this.cards = Object.keys(data).map((key) =>{
            return{
              id: key,
              ...data[key],
            };
          });
        } catch(e){
          this.alert = {
            type:'danger',
            title: 'Error',
            text: e.message
          };
      }
    },
    async deleteCard(id){
            try {
            const name = this.cards.find((card) => card.id === id).cardName;
            await axios.delete(
            `https://todo-mkion-default-rtdb.europe-west1.firebasedatabase.app/todoCards/${id}.json`
            );
            this.cards = this.cards.filter((card) => card.id !== id);
        } 
            catch (e) {}
    },
    
  },
  mounted(){
    this.loadCards();
  }

}
</script>
<style>
h1 {
  color: #000;
  text-align: center;
}
</style>
