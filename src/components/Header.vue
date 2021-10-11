<template>
  <header class="py-3 shadow-sm">
      <div class="container-md d-flex flex-sm-row flex-column align-items-center justify-content-between">
          <h2 class="fw-bold">Where in the world?</h2>
          <div class="switch-container d-flex align-items-center" @click="changeMode">
              <i class="far fa-moon mx-2"></i>
              <p class="mode m-0 fw-bold d-block">Dark Mode</p>
          </div>
      </div>
  </header>
</template>

<script>
export default {
    name: 'Header',
    data(){
        return{

        }
    },
    methods:{
        changeMode(){
            document.body.classList.toggle('dark');
            document.querySelector('header').classList.toggle('dark')
            document.querySelectorAll('.card').forEach(item => {
                item.classList.toggle('dark')
            })
            document.querySelector('.form-select').classList.toggle('dark')
            document.querySelector('.form-control').classList.toggle('dark')
            document.querySelector('.input-group-text').classList.toggle('dark')
            document.querySelector('.country-detail').classList.toggle('dark')
        }
    }
}
</script>

<style scoped>
    .switch-container{
        cursor: pointer;
    }
</style>