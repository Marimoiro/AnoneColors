<template>
  <q-page class="row items-center justify-evenly">

    <q-card>
    <q-tab-panels v-model="tab">
      <q-tab-panel v-for="pattern in patterns" :key="pattern.title" :name="pattern.index" :label="pattern.title">

          <q-card class="full-width ">
            <q-card-section>
              <q-input v-model="pattern.title" dense :maxlength="30"></q-input>
            </q-card-section>
            <q-separator> </q-separator>
            <q-card-section>
                <div class="row items-center  justify-evenly">
                  <q-card class="col-10" style="min-height: 300px; min-height:400px; height: 400px;width: 300px">
                    <pattern-circle :base-color="pattern.baseColor" :sub-color="pattern.subColor" :title="pattern.title" :isDark="darkMode == 'dark'"></pattern-circle>
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
        <q-tab v-for="pattern in patterns" :key="pattern.title" :name="pattern.index" :label="pattern.title">
          {{ pattern.baseColor}} / {{ pattern.subColor }}
        </q-tab>
        <q-tab name="add" @click="addPattern" >
          <q-icon name="add" />
        </q-tab>
      </q-tabs>
      <q-card id="colorPicker">
        <q-card-section>
          <q-tabs v-model="patternType">
            <q-tab name="baseColor" label="Base Color">
            </q-tab>
            <q-tab name="subColor" label="Sub Color">
            </q-tab>
          </q-tabs>

        </q-card-section>
        <q-card-section>
          <color-selector v-for="c in 21" :key="c" :color="'brand-' + (c + 1).toString()" :index="tab" :onSelect="() => onSelect(c)" />
        </q-card-section>
      </q-card>
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
  subColor: string
  index: number = 0

  constructor(title: string, baseColor: string, subColor: string,index: number = 0) {
    this.title = title
    this.baseColor = baseColor
    this.subColor = subColor
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
  new Pattern('カラーパターン', colors[0], 'blue',0),
  ]
)

function addPattern() {
  patterns.push(new Pattern('カラーパターン', colors[0], 'blue', patterns.length))
  tab.value = patterns.length - 1
}

function selectedPattern() {
  return patterns[tab.value]
}

function onSelect(c: number) {
  if (patternType.value == "baseColor") {
    selectedPattern().baseColor = colors[c]
  } else {
    selectedPattern().subColor = colors[c]
  }
}

const $q = useQuasar()
function setLightMode(isDark: boolean) {

  $q.dark.set(isDark)
}
</script>
