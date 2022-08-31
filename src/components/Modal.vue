<template>
   <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header">default header</slot>
          </div>

          <div class="modal-body">
            <slot name="body">default body</slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
               <button
                class="modal-default-button no__btn"
                @click="$emit('close')"
              >Нет</button>
              <button
                class="modal-default-button yes__btn"               
                @click="$emit('deleteCard')"
              >Да</button>
              
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
 props: {
    show: Boolean
  }
}
</script>

<style>
.yes__btn{
    border: 0;
    background: red;
    color: #fff;
    padding: 5px;
    border-radius: 3px;
    width: 35px;
}
.no__btn{
    border: 0;
    background: green;
    color: #fff;
    padding: 5px;
    border-radius: 3px;
}
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 40px 50px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: red;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
  margin-right: 7px;
  cursor: pointer;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>