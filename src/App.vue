<template>
  <div id="app">
    <header>
      <nav>
        <ul>
          <li><router-link to="/" @click="changeColor(0)" :class="{ active: activeLink === 0 }">Weather</router-link></li>
          <li><router-link to="/location" @click="changeColor(1)" :class="{ active: activeLink === 1 }">Location</router-link></li>
          <li><router-link to="/stopwatch" @click="changeColor(2)" :class="{ active: activeLink === 2 }">Stopwatch</router-link></li>
          <li><router-link to="/snakegame" @click="changeColor(3)" :class="{ active: activeLink === 3 }">Snake Game</router-link></li>
          <li><router-link to="/photo" @click="changeColor(4)" :class="{ active: activeLink === 4 }">Photos</router-link></li>
        </ul>
      </nav>
    </header>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      activeLink: null
    };
  },
  methods: {
    changeColor(index) {
      this.activeLink = index;
    }
  }
};
</script>

<style>
#app {
  text-align: center;
}

header {
  background: #4b514f;
  padding: 20px;
}

nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

nav li {
  display: inline-block;
  margin-right: 10px;
}

nav a {
  color: #fff;
  text-decoration: none;
  padding: 5px 10px;
}

nav a:hover {
  text-decoration: underline;
}

nav .active {
  background-color: #222320;
}
</style>
