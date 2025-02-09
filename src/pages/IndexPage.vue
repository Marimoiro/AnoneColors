<template>
  <q-page class="row items-center justify-evenly">

    <q-card>
    <q-tab-panels v-model="tab">
      <q-tab-panel v-for="pattern in patterns" :key="pattern.title" :name="pattern.index" :label="pattern.title">

          <q-card class="full-width ">
            <q-separator> </q-separator>
            <q-card-section>
                <div class="row items-center  justify-evenly">
                  <q-card class="col-10" style="min-height: 300px; min-height:400px; height: 400px;width: 300px">
                    <pattern-circle :base-color="pattern.baseColor" :base-color-index="pattern.baseColorIndex" :sub-color="pattern.subColor" :sub-color-index="pattern.subColorIndex" :title="pattern.title" :isDark="darkMode == 'dark'"></pattern-circle>
                  </q-card>
                </div>
              </q-card-section>


            </q-card>

      </q-tab-panel>
    </q-tab-panels>
      <q-separator></q-separator>
      <q-tabs v-model="darkMode">
        <q-tab name="light" label="Light" @click="setLightMode(false)"></q-tab>
        <q-tab name="dark" label="Dark" @click="setLightMode(true)"></q-tab>
      </q-tabs>
      <q-separator />
      <q-tabs
        v-model="tab"
        dense
        align="justify"
        narrow-indicator
      >
        <q-tab v-for="pattern in patterns" :key="pattern.title" :name="pattern.index">
          {{ pattern.baseColorIndex }} / {{ pattern.subColorIndex }}
        </q-tab>
        <q-tab name="add" @click="addPattern" >
          <q-icon name="add" />
        </q-tab>
      </q-tabs>
      <q-card id="colorPicker">
        <q-card-section>
          <q-tabs v-model="patternType">
            <q-tab name="baseColor" label="ベースカラー">
            </q-tab>
            <q-tab name="subColor" label="ラインカラー">
            </q-tab>
          </q-tabs>

        </q-card-section>
        <q-card-section>
          <color-selector v-for="c in 21" :key="c" :color="'brand-' + (c).toString()" :index="tab" :onSelect="() => onSelect(c)" />
        </q-card-section>
        <q-card-section>
       </q-card-section>
      </q-card>
      </q-card>

      <q-card class="q-ma-sm q-pa-md">
        使い方
        <li>
          ベースカラーとラインカラーを選択してください。合わせて画像ができます。
        </li>
        <li>
          良いのができたらDownloadボタンからDLしてください。直接DLはできないです。
        </li>
        <li>
          Dark, Lightで背景色が変わります。合わせるためののイメージにどうぞ。
        </li>
        <li>
          +ボタンでパターンを増やせます。
        </li>
        <li>
          保存されたりしないので、リロードしたら全部消えます。
        </li>

        注意事項
        <li>
          このアプリは、ジャージ用カラーパターンを作成するためのファンメイドページです。
        </li>
        <li>
          元の画像から色をとってきていますが、実際の色とは異なります。あくまでイメージです。
        </li>
        <li>
          このページで作成したものについては一切の責任を負いません。
        </li>
        <li>
          数時間で作った間に合わせなので不備とかあっても許してね。
        </li>
        <li>
          怒られたら消します。
        </li>
      </q-card>
      </q-page>
</template>

<script setup lang="ts">

import {reactive, ref} from 'vue';
import ColorSelector from 'pages/ColorSelector.vue';
import PatternCircle from 'pages/PatternCircle.vue';
import {useQuasar} from 'quasar';

const tab = ref(0)
const patternType = ref("baseColor")
const darkMode = ref("light")

class Pattern {
  title: string
  baseColor: string
  baseColorIndex: number
  subColor: string
  subColorIndex: number
  index: number = 0

  constructor(title: string, baseColor: string, subColor: string,index: number = 0,baseColorIndex: number = 0, subColorIndex: number = 1) {
    this.title = title
    this.baseColor = baseColor
    this.baseColorIndex = baseColorIndex
    this.subColor = subColor
    this.subColorIndex = subColorIndex
    this.index = index
  }

}

//const colors = ["red", "blue", "green", "yellow", "purple", "orange", "pink", "cyan", "teal", "lime", "brown", "grey", "black", "white"]

const colors = [
  "#0e0e0e",
  "#efefef",
  "#A70b00",
  "#6D0B1A",
  "#FF8657",
  "#F2C242",
  "#542405",
  "#9DA4B6",
  "#EDA6C6",
  "#DB4486",
  "#B47A88",
  "#B98BB9",
  "#370869",
  "#CFB1A5",
  "#072887",
  "#87AFD1",
  "#7ECED5",
  "#0C727F",
  "#C5DA57",
  "#1C6934",
  "#17494A"
]

const patterns : Pattern[] = reactive([
  new Pattern('カラーパターン', colors[0], colors[14],0,1,15),
  ]
)

function addPattern() {
  patterns.push(new Pattern('カラーパターン', colors[0], colors[14], patterns.length,1,15))
  tab.value = patterns.length - 1
}

function selectedPattern() {
  return patterns[tab.value]
}

function onSelect(c: number) {
  const pattern = selectedPattern()
  if (patternType.value == "baseColor") {
    pattern.baseColor = colors[c-1]
    pattern.baseColorIndex = c
  } else {
    pattern.subColor = colors[c-1]
    pattern.subColorIndex = c
  }
}

const $q = useQuasar()
function setLightMode(isDark: boolean) {

  $q.dark.set(isDark)
}
</script>
