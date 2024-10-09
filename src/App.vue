<script setup lang="ts">
import { ref } from "vue";
import { Icon } from "@iconify/vue";

const currentRotate = ref<number>(0);
const itemsCount = ref(20);
const currentItem = ref<number>(0);
const size = ref(13);

window.document.documentElement.style.overflow = "scroll";

function rotate(selectedItem: number) {
  currentItem.value = selectedItem;
  const itemRelativeDegrees = (360 / itemsCount.value) * selectedItem;
  const multiplier = Math.ceil(
    (itemRelativeDegrees + currentRotate.value) / 360
  );

  const positioned =
    360 * multiplier - itemRelativeDegrees - currentRotate.value;
  currentRotate.value +=
    positioned > 180
      ? 360 * (multiplier - 1) - itemRelativeDegrees - currentRotate.value
      : positioned;
}
</script>

<template>
  <section
    class="bg-dark-950 min-h-svh w-full bg-white flex items-center justify-center"
  >
    <div class="relative md:w-[600px] md:h-[600px] w-full aspect-square">
      <div
        class="absolute z-20 left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 text-center"
      >
        <div>Click on any image</div>
        <div class="flex items-center justify-center gap-3 pt-3 text-2xl">
          <Icon
            icon="gala:remove"
            class="text-3xl cursor-pointer"
            @click="itemsCount--"
          />
          {{ itemsCount }}
          <Icon
            icon="gala:add"
            class="text-3xl cursor-pointer"
            @click="itemsCount++"
          />
        </div>
        <div class="flex items-center justify-center gap-3 pt-3 text-2xl">
          <Icon
            icon="gala:remove"
            class="text-3xl cursor-pointer"
            @click="size--"
          />
          {{ size }}
          <Icon
            icon="gala:add"
            class="text-3xl cursor-pointer"
            @click="size++"
          />
        </div>
        <div class="pt-3">
          Sin: 360 / {{ itemsCount }} * {{ currentItem }} + {{ itemsCount }} / 2
        </div>
        <div>
          Cos: 360 / {{ itemsCount }} * {{ currentItem }} + {{ itemsCount }} / 2
        </div>
      </div>
      <div class="absolute -left-14 top-1/2 -translate-y-16">
        <Icon icon="mynaui:fat-arrow-right" class="text-slate-500 text-5xl" />
      </div>
      <div
        class="absolute flex h-full w-full items-center justify-center gap-3 rotate-[9deg]"
      >
        <div class="absolute bottom-[1%] left-[1%] right-[1%] top-[1%]">
          <div
            class="absolute z-10 rounded-full border-4 border-[#000]"
            :style="{
              width: `${size}%`,
              height: `${size}%`,
              left: `calc( ${50 - size/2}% + cos((360 / ${itemsCount} * ${itemsCount / 2}
                  ) * pi / 180) * (43.5%))`,
              top: `calc( ${50 - size/2}% + sin((360 / ${itemsCount} * ${itemsCount / 2}
                  ) * pi / 180) * 43.5%)`,
            }"
          ></div>
        </div>
        <div
          class="absolute bottom-[1%] left-[1%] right-[1%] top-[1%] transition-all ease-in-out duration-[3s]"
          :style="{
            transform: `rotate(${currentRotate}deg)`,
          }"
        >
          <div
            v-for="(_, i) in Array.from({ length: itemsCount })"
            @click="rotate(i)"
            :class="[
              `absolute cursor-pointer transition-all ease-in-out duration-[3s] shadow-lg border-4 border-[#fff] rounded-full`,
            ]"
            :style="{
              width: `${size}%`,
              height: `${size}%`,
              transform: `rotate(calc(${(currentRotate + 9) * -1}deg))`,
              left: `calc(${50 - size/2}% + cos((360 / ${itemsCount} * ${
                i + itemsCount / 2
              }) * pi / 180) * (43.5%))`,
              top: `calc(${50 - size/2}% + sin((360 / ${itemsCount} * ${
                i + itemsCount / 2
              }) * pi / 180) * 43.5%)`,
            }"
          >
            <div :class="[currentItem == i && 'curraent-item']">
              <img
                class="rounded-full md:rotate-0"
                :src="`https://xsgames.co/randomusers/avatar.php?g=female&n=${
                  i + 1
                }`"
                alt=""
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@keyframes selectedAnimation {
  0% {
    transform: scale(1) translateX(0);
  }
  100% {
    transform: scale(1.2) translateX(200px);
  }
}

.current-item {
  animation: selectedAnimation 1s backwards 700ms;
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
