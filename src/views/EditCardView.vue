<template>
   <edit-card
   :cardsList="cardsList"
   :cardId="cardId"
   @loadCards="loadCards"
   @addInput="addInput"
   ></edit-card>
</template>

<script>
import EditCard from '@/components/editCard.vue'
import axios from "axios";
export default {
  components: { EditCard },
    data(){
        return{
          cardsList: [],
          cardLength: 0
        }
    },
computed:{
        cardId(){
            return this.$route.params.id
        }
    },
    methods:{
         loadCards(){
      const url = 'https://todo-mkion-default-rtdb.europe-west1.firebasedatabase.app/todoCards/' + this.cardId + '.json'
        axios.get(url).then(response => {
            this.cardsList = response.data
          })                       
        },
        addInput(){
          let count = this.cardsList.task.length 
          this.cardsList.task.push({
            inpid: `li${count++}`,
            value: 'Введите название',
            isCheck: false,
          })
        },
        
    },
    mounted(){
        this.loadCards()

    },

}
</script>

<style>

</style>