<template lang="pug">
div(class="p-5")
  div(class="items-center mb-2 md:flex")
    input(
      class="border-solid border-[1px] p-1 w-full border-purple-300 mr-2 md:w-1/2",
      type='text',
      v-model='searchWord'
    )
    div(
      class="flex-none p-1 bg-purple-600 border-2 border-purple-600 text-white cursor-pointer",
      class="hover:text-purple-600 hover:bg-white"
    )
      div.search(@click="searchStream()")
        p 搜尋直播名稱
  div(class="flex flex-wrap justify-between bg-gray-400 p-2")
    div(
      v-for="video in nowList",
      class="m-2 p-2 flex-none bg-white max-w-[320px]",
      v-if="nowList.length > 0"
    )
      a(:href="video.videoHref", target="_blank")
        img(:src="video.videoImg")
        p {{ video.videoTitle }}
    p(v-else) 查無紀錄
</template>

<script setup>
useHead({
  title: '艾黎的直播記錄'
})

const searchWord = ref('');
const streamList = useNuxtApp().$streamList;
const nowList = ref(streamList);

const searchStream = () => {
  console.log('searchStream');
  const findVideo = useFilter(streamList, (video) => {
    if (useGet(video, 'videoTitle').match(new RegExp(searchWord.value, 'i'))) {
      return true;
    }
    return false;
  });
  nowList.value = findVideo;
  console.log(findVideo);
}
</script>