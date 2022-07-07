<template>
  <div class="flex justify-center">
    <div class="flex justify-center flex-col justify-items-center items-center">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 text-green-500 text-center" viewBox="0 0 20 20" fill="currentColor">
        <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
      </svg>

      <h2 class="mt-6 text-xl text-center">
        <strong>
          Успешно!
        </strong>
      </h2>
      <p class="mt-2 text-lg text-center px-6 block">Вы успешно провели тестирование вашего WebView!</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "DoneView"
}
</script>

<style scoped>

</style>