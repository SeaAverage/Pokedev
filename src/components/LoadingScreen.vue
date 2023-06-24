<template>
    <div id="loading">
      <img
        id="loadingImage"
        class="img"
        src="../../src/assets/poke_ball.png"
        alt="loading..."
        width="200"
      />
    </div>
  </template>
  

<style>
#loading {
  position: absolute;
  z-index: 1000;
  text-align: center;
  background-color: var(--color-background);
  width: 50vw;
  height: 100vh;
}
#loadingImage {
  position: relative;
  margin: auto;
  animation: rotation 2s infinite linear
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
</style>