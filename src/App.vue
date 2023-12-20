<template>
  <div class="app-container">
    {{ helloWorld.value }}
    <div class="date">Date: {{ date }}</div>
   <p> Day: {{ getDay() }}, Month: {{ getMonth() }}</p>
    <div class="input">
      <input type="text" v-model="input" @input="handleChange" />
      <button @click="changeTheWorld">Change The World</button>
    </div>
  </div>
</template>

<script>
import useHelloWorld from './components/useHelloWorld'; // Adjust the path accordingly

export default {
  setup() {
    const helloWorld = useHelloWorld();

    const data = {
      posts: [],
      input: '',
      date: new Date(),
    };

    const methods = {
      async fetchData() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/todos?limit=5');
          const data = await response.json();
          console.log(data);
          data.posts = data;
        } catch (error) {
          console.error(error);
        }
      },
      handleChange(event) {
        data.input = event.target.value;
      },
      changeTheWorld() {
        helloWorld.setValue(data.input);
      }
    };

    const computed = {
      getDay() {
        return data.date.getDate();
      },
      getMonth() {
        return data.date.getMonth();
      },
    };

    const mounted = () => {
      methods.fetchData();
    };

    return {
      helloWorld,
      ...data,
      ...methods,
      ...computed,
      mounted,
    };
  },
};
</script>

<style scoped>
    .app-container{
        display: flex;
        flex-direction: column;
        background-color: rgb(207, 207, 207);
        width: 50%;
        margin: 0 auto;
        align-items: center;
        border-radius: 5px;
    }
    .input, .date{
      margin-block: 20px;
    }
    .input input{
      width: 200px;
      height: 20px;
      border-top-left-radius: 5px;
      border-bottom-left-radius: 5px
    }
    .input button{
      width: 130px;
      height: 26px;
      border-top-right-radius: 5px;
      border-bottom-right-radius: 5px
    }
</style>