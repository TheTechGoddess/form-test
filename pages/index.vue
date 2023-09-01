<template>
  <div
    class="md:py-10 md:px-20 mx-5 -mt-20 md:mx-0 md:mt-0 rounded-lg bg-white md:bg-none px-6 py-8 self-center absolute md:relative"
  >
    <h1 class="text-4xl text-[#022857] font-bold">Personal info</h1>
    <p class="text-[#B9BABE] mt-2 font-medium">
      Please provide your name, email address, and phone number
    </p>
    <form action="" class="mt-10" @submit.prevent="submitForm">
      <div class="w-full flex flex-col mb-3">
        <label for="name" class="mb-2 text-[#002957] font-medium">Name</label>
        <input
          type="text"
          id="name"
          v-model="formData.name"
          class="px-4 mb-1 py-3.5 w-50% placeholder-[#9F9FA7] font-bold text-[#022857] border border-[#D9D8DD] rounded-md focus:outline-none focus:border-[#534D95] focus:ring-0"
          placeholder="e.g. Steven King"
          :class="{ 'border-red-500': showValidation && !formData.name }"
        />
        <!-- Display validation error message if the name is empty -->
        <p v-if="showValidation && !formData.name" class="text-red-500">
          Name is required
        </p>
      </div>
      <div class="w-full flex flex-col mb-3">
        <label for="email" class="mb-2 text-[#002957] font-medium"
          >Email Address</label
        >
        <input
          type="email"
          id="email"
          v-model="formData.email"
          class="mb-1 px-4 py-3.5 w-50% placeholder-[#9F9FA7] font-bold text-[#022857] border border-[#D9D8DD] rounded-md focus:outline-none focus:border-[#534D95] focus:ring-0"
          placeholder="e.g. stevenking@lorem.com"
          :class="{
            'border-red-500':
              showValidation &&
              (!formData.email || !isValidEmail(formData.email)),
          }"
        />
        <!-- Display validation error message if the email is invalid -->
        <p
          v-if="
            showValidation && (!formData.email || !isValidEmail(formData.email))
          "
          class="text-red-500"
        >
          Invalid email address
        </p>
      </div>
      <div class="w-full flex flex-col mb-3">
        <label for="phone" class="mb-2 text-[#002957] font-medium"
          >Phone Number</label
        >
        <input
          type="tel"
          id="phone"
          v-model="formData.phone"
          class="mb-1 px-4 py-3.5 w-50% placeholder-[#9F9FA7] font-bold text-[#022857] border border-[#D9D8DD] rounded-md focus:outline-none focus:border-[#534D95] focus:ring-0"
          placeholder="e.g. +1 234 567 890"
          :class="{ 'border-red-500': showValidation && !formData.phone }"
        />
        <!-- Display validation error message if the phone number is empty -->
        <p v-if="showValidation && !formData.phone" class="text-red-500 mb-3">
          Phone number is required
        </p>
      </div>
      <div class="mt-10 flex flex-col w-full justify-end">
        <button
          type="submit"
          class="bg-[#174A89] text-white w-32 rounded py-3 font-medium self-end"
        >
          Next Step
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  layout: "default",
  data() {
    return {
      formData: {
        name: "",
        email: "",
        phone: "",
      },
      showValidation: false, // Flag to track if validation should be shown
    };
  },
  methods: {
    isValidEmail(email) {
      // Basic email validation using a regular expression
      const emailPattern = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
      return emailPattern.test(email);
    },
    submitForm() {
      // Set the showValidation flag to true when the form is submitted
      this.showValidation = true;

      if (!this.formData.name || !this.formData.email || !this.formData.phone) {
        alert("Please fill out all required fields.");
        return;
      }

      // Form is valid, save the data to local storage
      localStorage.setItem("formData", JSON.stringify(this.formData));

      // Proceed to the next step
      this.$router.push("/plan");
    },
  },
};
</script>
