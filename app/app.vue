<template>
  <UContainer>
    <UCard class="mt-10 mb-10">
      <template #header>
        <div class="flex justify-between">
          <h1>Тестирование Green API</h1>
          <ColorScheme><USelect v-model="$colorMode.preference" :options="['system', 'light', 'dark']" /></ColorScheme>
        </div>
      </template>
    </UCard>

  <div class="grid grid-cols-2 gap-1 h-full">
    
    <div class="w-50% ml-100">
      <UCard class="mt-1 mb-1">
        <div class="flex gap-x-3 items-center">
          <UInput v-model="idInstance" color="white" variant="outline" placeholder="idInstance" class="w-full"/>
        </div>
        <div class="flex gap-x-3 items-center mt-3"> 
          <UInput v-model="apiTokenInstance" color="white" variant="outline" placeholder="apiTokenInstance" class="w-full"/>
        </div>
      </UCard>
      <UCard class="mt-1 mb-1">
        <div class="flex gap-x-3 items-center mt-3">
          <UButton color="primary" variant="solid" block @click="getGreenData('getSettings')">getSettings</UButton>
        </div>
        <div class="flex gap-x-3 items-center mt-3">
          <UButton color="primary" variant="solid" block @click="getGreenData('getStateInstance')">getStateInstance</UButton>
        </div>
      </UCard>
      <UCard class="mt-1 mb-1">
        <div class="flex gap-x-3 items-center mt-3"> 
          <UInput v-model="phoneNumberForMessage" color="white" variant="outline" placeholder="Номер телефона с кодом страны без '+'" class="w-full"/>
        </div>
        <div class="flex gap-x-3 items-center mt-3"> 
          <UTextarea v-model="messageText" class="w-full" placeholder="Текст сообщения"/>
        </div>        
        <div class="flex gap-x-3 items-center mt-3">
          <UButton color="primary" variant="solid" block @click="sendMessage()">sendMessage</UButton>
        </div>
      </UCard>
      <UCard class="mt-1 mb-1">
        <div class="flex gap-x-3 items-center mt-3"> 
          <UInput v-model="phoneNumberForURL" color="white" variant="outline" placeholder="Номер телефона с кодом страны без '+'" class="w-full"/>
        </div>
        <div class="flex gap-x-3 items-center mt-3"> 
          <UInput v-model="fileByURL" class="w-full" placeholder="URL файла для отправки. Пример: https://example.com/test.jpg"/>
        </div>  
        <div class="flex gap-x-3 items-center mt-3">
          <UButton color="primary" variant="solid" block @click="sendFileByURL()">sendFileByURL</UButton>
        </div>
      </UCard>      
    </div>

    <div class="w-50% h-full">
      <UCard class="mt-1 mb-1 h-full">
        <div>Ответ:</div>
        <div> 
          {{ dataFetch }}
        </div>
      </UCard>
    </div>
  </div> 
  </UContainer>
</template>

<script setup lang="ts">
  
  const apiUrl = 'https://1103.api.green-api.com';
  const mediaUrl = 'https://1103.api.green-api.com';
  
  const idInstance = ref('');
  const apiTokenInstance = ref('');
  const phoneNumberForMessage = ref('');
  const phoneNumberForURL = ref('');
  const messageText = ref('');
  const fileByURL = ref('');
  const dataFetch = ref('');

  async function getGreenData(queryType: string) {
    const url = apiUrl + '/waInstance' + idInstance.value + '/' + queryType +'/' + apiTokenInstance.value
    try {
      const respose = await $fetch(url);
      dataFetch.value = respose;
    }  catch (error) {
      dataFetch.value = error;
    }

  }

  async function sendMessage() {
    const url = apiUrl + '/waInstance' + idInstance.value + '/sendMessage/' + apiTokenInstance.value;
    try {
      const respose = await $fetch(url, {
        method: 'POST',
        body: {
          "chatId": phoneNumberForMessage.value + '@c.us', 
          "message": messageText.value
        }
      });
      dataFetch.value = respose;
    }  catch (error) {
      dataFetch.value = error;
    }
  }

  async function sendFileByURL() {
    const url = apiUrl + '/waInstance' + idInstance.value + '/sendFileByUrl/' + apiTokenInstance.value;
    try {   
      const respose = await $fetch(url, {
        method: 'POST',
        body: {
          "chatId": phoneNumberForURL.value + '@c.us', 
          "urlFile": fileByURL.value, 
          "fileName": fileByURL.value.split('/').pop().replace(/\s/g, "_"), 
          "caption": "Файл по URL"
        }
      });
      dataFetch.value = respose;
    }  catch (error) {
      dataFetch.value = error;
    }
  }
  
</script>
