<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
</template>

<script lang="ts">
import { defineComponent, toRefs, PropType } from 'vue'
import HelloWorld from './components/HelloWorld.vue'

type HelloWorldType = typeof HelloWorld
type propsNow = HelloWorldType['props']

type Props<T> = { [K in keyof T]: { type: PropType<T[K]> } }

// Hack. But there is not default or required
type actualProps = Props<Parameters<NonNullable<HelloWorldType['setup']>>[0]>

export default defineComponent({
  name: 'App',
  components: {
    HelloWorld
  },
  props: {
    // Try to remove `as actualProps`. Props will be type of `any`
    ...HelloWorld.props as actualProps,
    test: String
  },
  setup (props) {
    const HelloWorldProps = toRefs(props)

    console.log(HelloWorldProps.moreProps) // Date[] | undefined  // OK
    console.log(HelloWorldProps.required) // number[] | undefined // Should be number[]

    return {
      HelloWorldProps
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
