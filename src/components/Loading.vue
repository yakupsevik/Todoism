<script>
import { AtomSpinner } from "epic-spinners";
export default {
  name: "Loading",
  components: {
    AtomSpinner,
  },
};
</script>

<template>
  <Transition>
    <main>
      <div>
        <atom-spinner
          :animation-duration="1000"
          :size="60"
          color="#fff"
          style="margin: 0 auto"
        />
      </div>
    </main>
  </Transition>
</template>

<style lang="scss" scoped>
main {
  background-image: linear-gradient(rgb(255, 0, 106), rgb(255, 0, 128));
  background-repeat: no-repeat;
  width: 100vw;
  height: 100vh;
  display: grid;
  place-items: center;
  position: absolute;
  z-index: 999999999999999;
}
</style>