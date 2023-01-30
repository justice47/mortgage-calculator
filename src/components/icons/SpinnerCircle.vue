<template>
  <div :class="$style.loader">
    <svg
      :class="$style.circular"
      viewBox="25 25 50 50"
    >
      <circle
        :class="$style.path"
        cx="50"
        cy="50"
        r="20"
        fill="none"
        stroke="currentColor"
        stroke-width="4"
        stroke-miterlimit="10"
      />
    </svg>
  </div>
</template>

<style lang="scss" module>
  @keyframes rotate {
    100% {
      transform: rotate(360deg);
    }
  }

  @keyframes dash {
    0% {
      stroke-dasharray: 1, 200;
      stroke-dashoffset: 0;
    }

    50% {
      stroke-dasharray: 89, 200;
      stroke-dashoffset: -35;
    }

    100% {
      stroke-dasharray: 89, 200;
      stroke-dashoffset: -124;
    }
  }

  .loader {
    position: relative;
    width: 24px;
    height: 24px;
  }

  .circular {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    animation: rotate 2s linear infinite;
  }

  .path {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
    animation: dash 1.5s ease-in-out infinite;
    stroke-linecap: round;
  }
</style>
