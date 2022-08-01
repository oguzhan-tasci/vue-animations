<template>
  <div class="home">
    <Toast v-if="showToast" />
    <Todos @badValue="triggerToast" />
    <transition name="fade">
      <div v-if="showP">hello, ninjas</div>
    </transition>
    <button @click="showP = !showP">toggle</button>
  </div>
</template>


<!-- 
  .enter-from , .enter-to , .enter-active :
    These allows us to control the animation or transition of the elements onto the page.

   .enter-from : That is a class applied to the element before it enters the browser and this is where we set the initial css state of the element.
   .enter-to : This is a class applied to the element as it enters the browser and this is where we set the final css state of the elements.
   .enter-active : This is a class that is applied while the element is transitioning from one state to another.

   .leave-from , .leave-to, .leave-active:
    This is going to be the sates before it exits the page right here.

    Same rules available for 'leave' properties.

    

-->

<script>
import { ref } from 'vue'
import Toast from '../components/Toast'
import Todos from '../components/Todos'

export default {
  components: { Toast, Todos },
  setup() {
    const showToast = ref(false)
    const showP = ref(false)

    const triggerToast = () => {
      showToast.value = true;
      setTimeout(() => showToast.value = false, 3000)
    }

    return { showToast, triggerToast, showP }
  }
}
</script>

<style>
  .fade-enter-from {
    opacity: 0;
  }
  .fade-enter-to {
    opacity: 1;
  }
  .fade-enter-active {
    transition: all 2s ease;
  }
  .fade-leave-from {
    opacity: 1;
  }
  .fade-leave-to {
    opacity: 0;
  }
  .fade-leave-active {
    transition: all 2s ease;
  }
</style>