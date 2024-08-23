<template>
  <transition
    name="modal-fade"
    @before-enter="beforeEnter"
    @enter="enter"
    @leave="leave"
  >
    <div
      v-if="modal"
      class="fixed z-50 inset-0 flex items-center justify-center overflow-hidden"
    >
      <div class="fixed inset-0 transition-opacity">
        <div class="absolute inset-0 bg-white blur-md opacity-25"></div>
      </div>

      <div
        class="bg-white rounded-lg text-left overflow-hidden shadow-lg shadow-slate-500/50 transform transition-all w-full max-w-xs sm:max-w-sm md:max-w-2xl"
      >
        <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
          <h3 class="text-lg leading-6 font-medium text-gray-900">
            Rules to Follow
          </h3>
          <div class="mt-2">
            <ul class="list-disc list-inside">
              <li>Rule 1: Be respectful and considerate.</li>
              <li>No offensive language or inappropriate behavior.</li>
              <li>Do not share personal information.</li>
              <li>No spamming or advertising.</li>
              <li>Follow all community guidelines.</li>
            </ul>
          </div>
        </div>
        <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
          <button
            type="button"
            class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-blue-600 text-base font-medium text-white hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 sm:ml-3 sm:w-auto sm:text-sm"
            @click="closeModal"
          >
            Accept
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    modal: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    closeModal() {
      this.$emit("close");
    },
    beforeEnter(el) {
      el.style.opacity = 0;
      el.style.transform = "scale(0.95)";
    },
    enter(el, done) {
      setTimeout(() => {
        el.style.transition = "opacity 0.3s, transform 0.3s";
        el.style.opacity = 1;
        el.style.transform = "scale(1)";
        done();
      }, 0);
    },
    leave(el, done) {
      el.style.transition = "opacity 0.3s, transform 0.3s";
      el.style.opacity = 0;
      el.style.transform = "scale(0.95)";
      setTimeout(() => {
        done();
      }, 300);
    },
  },
};
</script>

<style scoped>
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.modal-fade-enter,
.modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
</style>
