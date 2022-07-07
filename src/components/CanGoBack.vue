<template>
  <div>
    <div class="bg-white overflow-hidden shadow sm:rounded-lg">
      <div class="px-4 py-12 sm:p-6 flex justify-center content-center items-center">
        <div class="flex justify-center flex-col items-center p-4">
          <div class="flex">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 animate-bounce mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M7 16l-4-4m0 0l4-4m-4 4h18" />
            </svg>
          </div>

          <h5 class="font-bold tracking-tight text-gray-900 text-xl mt-3">Попробуйте нажать кнопку "Назад"</h5>

          <p class="text-center text-gray-500 lg:mx-auto text-sm mt-2">Если ваше приложение вылетело - вероятно оно настроено не правильно</p>
        </div>
      </div>
    </div>

    <div class="mt-4">
      <div class="flex items-center content-center">
        <img src="https://stackoverflow.com/favicon.ico" alt="" class="w-4 h-4"/>
        <a href="https://stackoverflow.com/questions/5312334/how-to-handle-back-button-in-activity" class="text-blue-500 ml-2 text-sm" target="_blank">How to handle back button in activity</a>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'CanGoBack',
  data() {
    return {
    
    }
  },
  methods: {
   
  },
  mounted() {
   
  }
}
</script>
