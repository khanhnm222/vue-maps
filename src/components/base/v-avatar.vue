<template>
  <avatar :size="size" :variant="variant" :name="name" :colors="colors" />
</template>

<script lang="ts">
import Avatar from "vue-boring-avatars";
export default {
  components: {
    Avatar,
  },
  props: {
    colors: {
      type: Array as any,
      default: () => ["#92A1C6", "#1c83c7", "#F0AB3D", "#27be84", "#db7b21"],
    },
    name: {
      type: String as any,
      default: "krata",
    },
    variant: {
      type: String as any,
      default: "beam",
    },
    size: {
      type: [String, Number] as any,
      default: 24,
    },
  },
};
</script>

<style>
.test {
  color: #92A1C6 #1c83c7 #F0AB3D #27be84 #db7b21;
}
</style>
