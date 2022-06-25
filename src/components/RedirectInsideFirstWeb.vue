<template>
<div class="bg-gray-100 sm:rounded-lg">
  <div class="px-4 py-5 sm:p-6">
    <h3 class="text-lg leading-6 font-medium text-gray-900">Вы видите этот экран? - это <span class="text-yellow-600	text-bold font-bold">нехорошо</span></h3>
    <div class="mt-2 max-w-xl text-sm text-gray-500">
      <p>Вас не перебросили на другую страницу, а просто поменяли адрес. А это значит, что показывать WebView не нужно.</p>

      <div class="mt-4">
        <h6>Когда <b class="text-green-400">нужно</b> показать WebView:</h6>

        <ul class="mt-2">
            <li class="text-xs">
                <span class="text-blue-500"> https://webviewcheck.<b>wfc.su</b>/ </span> > <span class="text-blue-500"> https://webviewcheck.<b>wireforce.org</b>/ </span>
            </li>
        </ul>
      </div>

      <div class="mt-4">
        <h6>Когда <b class="text-red-400">не</b> нужно показать WebView:</h6>

        <ul class="mt-2">
            <li class="text-xs">
                <span class="text-blue-500"> https://<b>wfc.su</b>/page_1 </span> > <span class="text-blue-500"> https://<b>wfc.su</b>/page_2 </span>
            </li>
        </ul>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'RedirectInsideFirstWeb'
}
</script>
