<template>
<div class="bg-white shadow overflow-hidden sm:rounded-lg">
  <div class="px-4 py-5 sm:px-6">
    <div class="flex">
        <div class="mb-4">   
            <div class="mx-auto flex-shrink-0 flex items-center justify-center h-12 w-12 rounded-full bg-red-100 sm:mx-0 sm:h-10 sm:w-10">
            <!-- Heroicon name: outline/exclamation -->
            <svg class="h-6 w-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" aria-hidden="true">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
            </svg>
            </div>
        </div>

        <div class="ml-4">
            <h3 class="text-lg leading-6 font-medium text-gray-900">
                <b>Ошибка!</b> Не пришли данные от AppsFlyer
            </h3>
            <p class="mt-1 max-w-2xl text-sm text-gray-500">
                Вы правильно открыли ссылку, но забыли добавить к ней параметры AppsFlyer в виде GET параметров
            </p>
        </div>
    </div>
  </div>
  <div class="border-t border-gray-200">
    <dl>
      <div class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
        <dt class="text-sm font-medium text-gray-500">Где вы ошиблись:</dt>
        <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">Вероятно, вы забыли добавить строчку параметров в функции: <div class="mt-2"><span class="font-mono bg-red-50 px-2 py-1 rounded-lg shadow text-xs">WebView.loadUrl(webViewUrl + "?" + <b>DATA_FROM_APPSFLYER</b>)</span></div></dd>
      </div>
      <div class="bg-white px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
        <dt class="text-sm font-medium text-gray-500">Какие данные пришли:</dt>
        <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
            <span v-if="location.search != ''">{{ location.search }}</span>
            <span v-else class="text-xs">НИЧЕГО</span>
        </dd>
      </div>
      <div class="bg-white px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
        <dt class="text-sm font-medium text-gray-500">Что делать:</dt>
        <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
            Попробуйте зарегистрировать свой исходник в AppsFlyer через <a href="https://appsflyer.wfc.su/" class="text-blue-500" target="_blank">https://appsflyer.wfc.su/</a>, поменяйте ключ в фукнции 
            <div class="my-2"><span class="font-mono bg-red-50 px-2 py-1 rounded-lg shadow text-xs">AppsFlyerLib.getInstance().init(<b>КЛЮЧ</b>, null, this)</span></div>
            и попробуйте заново. Если это не сработает, ищите ошибку в районе строк, которые отвечают за загрузку ссылки в WebView
        </dd>
      </div>
    </dl>
  </div>
</div>
</template>

<script>
export default {
  name: 'AppsFlyerError',
  data() {
    return {
        location
    }
  }
}
</script>