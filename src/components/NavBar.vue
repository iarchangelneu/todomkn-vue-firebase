<template>
  <div class="navbar">
    <router-link to="/" style="margin-right:15px">Список задач</router-link>
    <router-link to="/create">Создать задачу</router-link>
  </div>
</template>

<script>
export default {

}
</script>

<style>
.navbar{
    display: flex;
    justify-content: center;
}
a{
  color: #000;
  text-decoration: none;
}
.router-link-active{
  border-bottom: 1px solid green;
}
</style>