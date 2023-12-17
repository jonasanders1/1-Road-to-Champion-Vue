<template>
  <div class="container">
    <div class="header">
      <div class="input-container">
        <input type="text">
        <button>Check</button>
      </div>
    </div>
    <div class="weather-content">

    </div>
  </div>
</template>


<script>
export default {

}
</script>


<style>
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
}

.input-container {
  display: flex;
  gap: 10px;
}
input {
  border: 2px solid rgb(46, 112, 226);
  padding: 5px 10px;
  font-size: 1.1rem;
  border-radius: 7px;
  color: rgb(46, 112, 226);
  font-weight: 500;
}
button {
  border: none;
  padding: 5px 10px;
  background-color:  rgb(46, 112, 226);
  border-radius: 7px;
  color: white;
  font-size: 1.1rem;
}
input, button {
  outline: none;
}

.weather-content {
  background-color: rgb(245, 245, 245);
  flex: 3;
}
</style>