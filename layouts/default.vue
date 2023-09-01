<template>
  <div class="bg-[#f9fafb] w-full h-screen md:py-6 md:px-20 lg:px-48">
    <div class="md:flex bg-[#fff] md:p-4 md:rounded-2xl">
      <SideBar />
      <div class="bg-[#f9fafb] relative">
        <slot />
      </div>
    </div>
  </div>
</template>
<script>
import SideBar from "~/components/layout/SideBar.vue";
// import Content from "~/components/layout/Content.vue";
export default {
  name: "default",
  components: {
    SideBar,
    // Content,
  },
};
</script>
