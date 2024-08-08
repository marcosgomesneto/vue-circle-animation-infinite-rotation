<script setup lang="ts">
import { ref } from "vue";

const currentRotate = ref<number>(0);
const itemsCount = 20;
const currentItem = ref<number>(0);

function rotate(selectedItem: number) {
  currentItem.value = selectedItem;
  const itemRelativeDegrees = (360 / itemsCount) * selectedItem;
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
  <section class="bg-dark-950 min-h-svh w-full bg-white">
    <svg class="h-0">
      <defs>
        <clipPath id="circlePath" clipPathUnits="objectBoundingBox">
          <path
            d="M1 0.923C1 0.966 0.932 1 0.85 0.994C0.64 0.978 0.445 0.929 0.293 0.852C0.105 0.758 0 0.631 0 0.498C0 0.364 0.105 0.236 0.293 0.142C0.445 0.066 0.64 0.017 0.85 0.001C0.932 -0.005 1 0.029 1 0.071C1 0.112 0.932 0.145 0.85 0.154C0.721 0.168 0.6 0.201 0.505 0.249C0.374 0.315 0.3 0.404 0.3 0.498C0.3 0.591 0.374 0.681 0.505 0.747C0.6 0.795 0.721 0.828 0.85 0.842C0.932 0.851 1 0.884 1 0.925Z"
          />
        </clipPath>
      </defs>
    </svg>
    <div class="mx-auto w-full bg-core-purple-300">
      <div
        class="relative w-auto h-[100vw] md:h-auto bg-red-500 md:w-[50%] md:max-w-[300px] rotate-90 md:rotate-0 inline-block md:block origin-[right_center]"
        :style="{
          clipPath: 'url(#circlePath)',
        }"
      >
        <div class="md:h-0 md:w-full md:pb-[200%] w-[calc(100vw*0.5)]">
          <div
            class="absolute flex h-full items-center justify-center gap-3 overflow-hidden pr-[200%] rotate-[9deg]"
          >
            <div class="absolute bottom-[1%] left-[1%] right-[1%] top-[1%]">
              <div
                class="absolute h-[13%] w-[13%] z-10 rounded-full border-4 border-[#000]"
                :style="{
                  left: `calc(43.5% + cos((360 / ${itemsCount} * ${
                    itemsCount / 2
                  }
                  ) * pi / 180) * (43.5%))`,
                  top: `calc(43.5% + sin((360 / ${itemsCount} * ${
                    itemsCount / 2
                  }
                  ) * pi / 180) * 43.5%)`,
                }"
              ></div>
            </div>
            <div
              class="absolute bottom-[1%] left-[1%] right-[1%] top-[1%] transition-all ease-in-out duration-1000"
              :style="{
                transform: `rotate(${currentRotate}deg)`,
              }"
            >
              <div
                v-for="(_, i) in Array.from({ length: itemsCount })"
                @click="rotate(i)"
                :class="[`absolute h-[13%] w-[13%] cursor-pointer transition-all ease-in-out duration-1000 shadow-lg border-4 border-[#fff] rounded-full`]"
                :style="{
                  transform: `rotate(calc(${(currentRotate + 9) * -1}deg))`,
                  left: `calc(43.5% + cos((360 / ${itemsCount} * ${
                    i + itemsCount / 2
                  }) * pi / 180) * (43.5%))`,
                  top: `calc(43.5% + sin((360 / ${itemsCount} * ${
                    i + itemsCount / 2
                  }) * pi / 180) * 43.5%)`,
                }"
              >
              <div :class="[currentItem == i && 'curraent-item']">
                <img
                  class="rounded-full -rotate-90 md:rotate-0"
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

.current-item{
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
