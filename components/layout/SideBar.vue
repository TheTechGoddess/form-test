<template>
  <div
    class="bg-[url('~/assets/images/bg-sidebar-mobile.svg')] bg-[#483EFF] w-full md:bg-[url('~/assets/images/bg-sidebar-desktop.svg')] md:w-[270px] h-[172px] md:h-[568px] md:rounded-2xl px-10 mb:py-12 py-6"
  >
    <div class="flex justify-between px-4 -mt-4 md:mt-2 md:px-0 md:flex-col">
      <div
        v-for="(step, index) in steps"
        :key="index"
        class="mb-8 mt-10 md:mt-6"
      >
        <nuxt-link
          :to="step.path"
          class="flex"
          @click="activeStepIndex = index"
        >
          <div class="flex justify-between">
            <div
              class="flex flex-col justify-center items-center content-center mr-4"
              :class="[
                'progress-bar-annual',
                { active: index === activeStepIndex },
              ]"
            >
              <p class="text-[#214464] font-bold text-sm">
                {{ step.stepNumber }}
              </p>
            </div>
          </div>

          <div
            class="hidden md:flex flex-col justify-center items-center content-center -mt-1.5"
          >
            <p class="text-[#A8A7FF] capitalize uppercase text-sm self-start">
              {{ step.stepName }}
            </p>
            <p class="text-[#F7F6FF] font-semibold">
              {{ step.stepDescription }}
            </p>
          </div>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      activeStepIndex: 0,
      steps: [
        {
          path: "/",
          stepNumber: 1,
          stepName: "STEP 1",
          stepDescription: "YOUR INFO",
        },
        {
          path: "/plan",
          stepNumber: 2,
          stepName: "STEP 2",
          stepDescription: "SELECT PLAN",
        },
        {
          path: "/add-ons",
          stepNumber: 3,
          stepName: "STEP 3",
          stepDescription: "ADD ONS",
        },
        {
          path: "/summary",
          stepNumber: 4,
          stepName: "STEP 4",
          stepDescription: "SUMMARY",
        },
      ],
    };
  },
  computed: {
    activeStepIndex() {
      // Determine the active step index based on the current route path
      const currentPath = this.$route.path;
      if (currentPath.startsWith("/summary/")) {
        // If it starts with "/summary/", set the active step to 4
        return 3; // Index 3 corresponds to step 4
      }
      const activeStep = this.steps.find((step) => step.path === currentPath);
      return activeStep ? this.steps.indexOf(activeStep) : 0;
    },
  },
  methods: {
    isStepActive(step) {
      // Check if the provided step is the active step
      return this.steps.indexOf(step) === this.activeStepIndex;
    },
  },
};
</script>
<style scoped>
.progress-bar-annual {
  border: #fff 1px solid;
  color: white;
  background-color: #483eff;
  border-radius: 50%;
  position: relative;
  width: 32px;
  height: 32px;
}
.active {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: #bee2f8;
  position: relative;
}
</style>
