<script lang="ts" setup>
import { ref } from "vue";
import { useElementSize } from "@vueuse/core";

const transform = ref<string>("");
const touchCardRef = ref<HTMLElement>();
const { width, height } = useElementSize(touchCardRef);

const touchTransform = (x: number, y: number): void => {
  const rotateX = -(y / (height.value / 2) - 1) * 10;
  const rotateY = (x / (width.value / 2) - 1) * 10;
  console.log(x / (width.value / 2));
  transform.value = `perspective(2000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
};

const handleMouseOut = () =>
  (transform.value = "perspective(2000px) rotateX(0deg) rotateY(0deg)");

const handleMouseMove = (event: MouseEvent) => {
  const { layerX: x, layerY: y } = event;
  touchTransform(x, y);
};
</script>
<template>
  <div
    class="w-screen h-screen flex justify-center items-center bg-neutral-200"
  >
    <div
      class="w-1/2 h-60 relative z-1 transform-3d transition-transform flex items-center"
      :style="{ transform }"
    >
      <div
        ref="touchCardRef"
        class="w-full h-full backdrop-blur-[.58594rem] rounded-2xl border-1 border-white shadow-lg shadow-gray-400 flex flex-col justify-center card"
        @mousemove="handleMouseMove"
        @mouseout="handleMouseOut"
      >
        <div class="ml-20 text-4xl text-[#141413] font-bold">@小王饿了</div>
        <div class="ml-20 mt-6 text-xl text-sky-500 font-bold">
          #fun-widgets/TouchCard
        </div>
      </div>
      <img
        class="absolute pointer-events-none translate-z-1 w-1/4 -right-1/8 rounded-full border-1 border-white"
        src="https://p3-flow-imagex-sign.byteimg.com/user-avatar/f9e7748c4fea045de8a774d0836ad177~tplv-a9rns2rl98-image.jpeg?rk3s=98c978ad&x-expires=1743502443&x-signature=YLBy9quP5%2B3%2BsPEza3K4SfeRfNg%3D"
      />
    </div>
  </div>
</template>
<style lang="scss" scoped>
.card {
  backdrop-filter: blur(0.58594rem);
  background: linear-gradient(
    113deg,
    hsla(0, 0%, 100%, 0.72) 10.29%,
    hsla(0, 0%, 100%, 0) 100.35%
  );
}
.transition-transform {
  transition: transform 0.15s linear;
}
</style>
