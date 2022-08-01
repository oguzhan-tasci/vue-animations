<template>
  <div class="about">
    <transition
      appear
      @before-enter="beforeEnter"
      @enter="enter"
      :css="false"
    >
      <h1 v-if="showTitle">About</h1>
    </transition>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
  </div>
</template>

<script>
import { ref } from 'vue'
import gsap from 'gsap'

export default {
  setup() {
    const showTitle = ref(true)

    const beforeEnter = (el) => {
      console.log('before enter - set intial state')
      el.style.transform = 'translateY(-60px)'
      el.style.opacity = 0
    }

    // Gsap
    // That's where we use gsap because we want to animate it. 
    // In order to do that we'll use 'to' and this is us saying we want to animate 'to' something or other. 
    // It takes two arguments : First one is the element that we want to animate in and we get access to that ;
    // Second argument is an object and in here we define the different css properties that we want to animate.

    // done -> This is a function that we can invoke to tell view that we've done with the transition
    const enter = (el, done) => {
      console.log('starting to enter - make transition')
      gsap.to(el, {
        duration: 1,
        y: 0,
        opacity: 1,
        ease: 'bounce.out',
        onComplete: done
      })
    }

    return { beforeEnter, enter, showTitle }
  }
}
</script>

<style>
  .about {
    max-width: 600px;
    margin: 20px auto;
  }
</style>