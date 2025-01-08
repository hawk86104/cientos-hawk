<!--
 * @Description: 
 * @Version: 1.668
 * @Autor: 地虎降天龙
 * @Date: 2024-04-17 15:39:35
 * @LastEditors: 地虎降天龙
 * @LastEditTime: 2024-04-17 15:42:37
-->
<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { BasicShadowMap, SRGBColorSpace, NoToneMapping } from 'three'

import { OrbitControls, useProgress, Environment, Sphere } from '@tresjs/cientos'
import { TresLeches, useControls } from '@tresjs/leches'
import '@tresjs/leches/styles'

const gl = {
  clearColor: '#82DBC5',
  shadows: true,
  alpha: false,
  shadowMapType: BasicShadowMap,
  outputColorSpace: SRGBColorSpace,
  toneMapping: NoToneMapping,
}

const { background, blur } = useControls({
  background: true,
  blur: {
    value: 0,
    min: 0,
    max: 1,
    step: 0.01,
  },
}, {
  uuid: 'Lightformer',
})

const { progress, hasFinishLoading } = await useProgress()
</script>

<template>
  <Transition
    name="fade-overlay"
    enter-active-class="opacity-1 transition-opacity duration-200"
    leave-active-class="opacity-0 transition-opacity duration-200"
  >
    <div
      v-show="!hasFinishLoading"
      class="absolute bg-white t-0 l-0 w-full h-full z-20 flex justify-center items-center text-black font-mono"
    >
      <div class="w-200px">
        Loading... {{ progress }} %
        <i class="i-ic-twotone-catching-pokemon animate-rotate-in" />
      </div>
    </div>
  </Transition>
  <TresLeches
    class="top-0 important-left-4"
    uuid="Lightformer"
  />
  <TresCanvas v-bind="gl">
    <TresPerspectiveCamera :position="[7, 7, 7]" />
    <OrbitControls />
    <Suspense>
      <Environment
        :background="background.value"
        :blur="blur.value"
        preset="sunset">
        <TresGroup>
          <Lightformer :intensity="0.75" :rotation-x="Math.PI / 2" :position="[0, 5, -9]" :scale="[10, 10, 1]" />
          <Lightformer :intensity="4" :rotation-y="Math.PI / 2" :position="[-5, 1, -1]" :scale="[20, 0.1, 1]" />
          <Lightformer :rotation-y="Math.PI / 2" :position="[-5, -1, -1]" :scale="[20, 0.5, 1]" />
          <Lightformer :rotation-y="-Math.PI / 2" :position="[10, 1, 0]" :scale="[20, 11, 1]" />
        </TresGroup>
      </Environment>
    </Suspense>
    <Sphere>
      <TresMeshStandardMaterial
        color="yellow"
        :roughness="0"
        :metalness="0.5"
      />
    </Sphere>
    <TresAmbientLight :intensity="1" />
  </TresCanvas>
</template>
