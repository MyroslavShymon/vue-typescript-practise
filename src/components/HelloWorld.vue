<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <input type="text" name="" id="" v-model.number="firstNumber" />
      <h2>{{ firstNumber }}</h2>
      <button @click="increment()">Increment</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({
  data() {
    return {
      firstNumber: 0,
    };
  },
})
export default class HelloWorld extends Vue {
  private firstNumber!: number;
  @Prop({ type: String, default: "" })
  private msg!: string;

  increment(): number {
    return this.firstNumber++;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
