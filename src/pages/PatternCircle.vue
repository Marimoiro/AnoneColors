<script setup lang="ts">

import {Ref, ref} from 'vue';

const props = defineProps({
  baseColor: {
    type: String,
    default: 'white'
  },
  subColor: {
    type: String,
    default: 'black'
  },
  baseColorIndex: {
    type: Number,
    default: 0
  },
  subColorIndex: {
    type: Number,
    default: 1
  },
  title: {
    type: String,
    default: 'title'
  },
  isDark: {
    type: Boolean,
    default: false
  }

})

const svgElement : Ref<HTMLElement | null> = ref(null);


function downloadSvgAsPng() {
  // svg要素を取得
  const svgNode = svgElement.value;
  console.log(svgNode);
  const svgText = new XMLSerializer().serializeToString(svgNode!);
  console.log(svgText);
  const svgBlob = new Blob([svgText], { type: 'image/svg+xml;charset=utf-8' });
  const svgUrl = URL.createObjectURL(svgBlob);
  console.log(svgUrl)

  // Imageオブジェクトを生成
  const im = new Image();

  // Imageの作成に少し時間がかかるため、addEventListnerで行う
  im.addEventListener('load', () => {
    const width = svgNode!.getAttribute('width');
    const height = svgNode!.getAttribute('height');

    // canvasを作成
    const cvs = document.createElement('canvas');
    cvs.setAttribute('width', width!.toString());
    cvs.setAttribute('height', height!.toString());
    const ctx = cvs.getContext('2d');

    // canvasに描画(背景は透過)
    ctx!.drawImage(im, 0, 0, parseInt(width!), parseInt(height!));
    const imgUrl = cvs.toDataURL('image/png');

    // a要素を作ってダウンロード
    const a = document.createElement('a');
    a.href = imgUrl;
    a.download = `MyPng.png`;
    document.body.appendChild(a);
    a.click();
    //document.body.removeChild(a);
    URL.revokeObjectURL(svgUrl);
    URL.revokeObjectURL(imgUrl);
  });

  // Imageオブジェクトを、svgデータから作成
  im.src = svgUrl;
}

</script>



<template>

  <!-- 円を描く -->
  <svg width="300px" height="300px" viewBox="0 0 100 100" focusable="false" ref="svgElement">
    <circle cx="50" cy="50" r="40" :stroke="props.baseColor" stroke-width="2" :fill="props.baseColor" />
    <line x1="5" y1="105" x2="105" y2="5" :stroke="props.subColor" stroke-width="5" />
    <line x1="-5" y1="95" x2="95" y2="-5" :stroke="props.subColor" stroke-width="5" />
    <text x="50%" y="95" font-size="5" :fill='props.isDark  ? "white" : "black"' text-anchor="middle" alignment-baseline="middle" > 色番号: {{props.baseColorIndex}} / {{ props.subColorIndex }}</text>
  </svg>
  <q-btn @click="downloadSvgAsPng">Download</q-btn>

</template>

<style scoped>

/* cssで円を描く */
.circle {
  width: inherit;
  height: inherit;
  border-radius: 50% !important;
  background-color: #f00;
}

.circle::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(-45deg, transparent 49%, black 50%, transparent 51%);
}


</style>
