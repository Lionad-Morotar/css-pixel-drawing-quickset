<template>
<div id="app">
    
    <PixelGenerator msg="Welcome to Your Vue.js App"/>

</div>
</template>

<script>

import PixelGenerator from "./pages/PixelGenerator"

export default {
    name: "app",
    components: {
        PixelGenerator
    }
}

</script>

<style>

* {
    margin: 0;
    padding: 0;
}

#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    width: 100vw;
    height: 100vh;
}

</style>
