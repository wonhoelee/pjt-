<template>
  <div class="loader">
    <svg class="circular" viewBox="25 25 50 50">
      <circle class="path" cx="50" cy="50" r="20" fill="none" stroke-width="2" stroke-miterlimit="10" />
    </svg>
  </div>
</template>

<script>
export default {
  name: 'LoadingSpinner',
};
</script>

<style lang="scss">
.loader {
  position: relative;
  margin: 80px auto;
  width: 100px;
  &:before {
    content: '';
    display: block;
    padding-top: 100%;
  }

  .circular {
    -webkit-animation: rotate 1s linear infinite;
    animation: rotate 1s linear infinite;
    height: 100%;
    -webkit-transform-origin: center center;
    -ms-transform-origin: center center;
    transform-origin: center center;
    width: 100%;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    .path {
      stroke-dasharray: 1, 200;
      stroke-dashoffset: 0;
      -webkit-animation: dash 1s ease-in-out infinite;
      animation: dash 1s ease-in-out infinite;
      stroke-linecap: round;
      stroke: #016afd;
    }
  }
}

@-webkit-keyframes rotate {
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

@keyframes rotate {
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

@-webkit-keyframes dash {
  0% {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
  }
  50% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -35px;
  }
  100% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -124px;
  }
}

@keyframes dash {
  0% {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
  }
  50% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -35px;
  }
  100% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -124px;
  }
}
</style>
