<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>前端技术栈</h2>
    <ul>
      <li>
        <a
          href="https://vuejs.org"
          target="_blank"
        >
        Vue
        </a>
      </li>
      <li>
        <a
          href="https://reactjs.org/"
          target="_blank"
        >
          React
        </a>
      </li>
      <li>
        <a
          href="https://d3js.org/"
          target="_blank"
        >
          D3
        </a>
      </li>
      <li>
        <a
          href="https://www.webpackjs.com/"
          target="_blank"
        >
          Webpack
        </a>
      </li>
      <br>
    </ul>
    <h2>后端技术栈</h2>
    <ul>
      <li>
        <a
          href="https://nodejs.org/zh-cn/"
          target="_blank"
        >
          Node
        </a>
      </li>
      <li>
        <a
          href="https://spring.io/"
          target="_blank"
        >
          Spring
        </a>
      </li>
      <li>
        <a
          href="https://www.mysql.com/"
          target="_blank"
        >
          MySQL
        </a>
      </li>
      <li>
        <a
          href="https://www.mongodb.com/"
          target="_blank"
        >
          Mongodb
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '最爱的天边，赴一个约会'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  width: 100px;
  height: 30px;
  background-color: #42b983;
  border-radius: 10px;
  margin: 0 10px;
  opacity: 0.8;
  align-items: center;
}
a {
  /* padding-top: 30px; */
  text-decoration: none;
  cursor: pointer;
}
a:hover{
  color: black;
}
</style>
