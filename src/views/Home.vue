<template>
  <div class="calculator-wrapper">
    <div class="display"></div>
    <div class="one number">1</div>
    <div class="two number">2</div>
    <div class="three number">3</div>
    <div class="plus operator">+</div>
    <div class="four number">4</div>
    <div class="five number">5</div>
    <div class="six number">6</div>
    <div class="minus operator">-</div>
    <div class="seven number">7</div>
    <div class="eight number">8</div>
    <div class="nine number">9</div>
    <div class="multiply operator">*</div>
    <div class="equals">=</div>
    <div class="zero number">0</div>
    <div class="divide operator">/</div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.calculator-wrapper {
  background-color: #2e2727;
  display: grid;
  gap: 2rem;
  grid-template-columns: repeat (4, 1fr);
  grid-template-rows: repeat (5, 1fr);
  height: 50rem;
  max-width: 35rem;
  margin: 10rem auto;
  padding: 2rem;

  .display {
    background-color: #cecece;
    border-radius: 0.5rem;
    font-size: 3rem;
    grid-column: 1 / span 4;
    grid-row: 1;
    line-height: 1;
    min-height: 3.1rem;
  }

  .number,
  .operator,
  .equals {
    border-radius: 0.5rem;
    color: white;
    display: grid;
    font-size: 3rem;
    line-height: 1;
    place-items: center;
  }

  .number {
    background-color: #9a9a9a;
  }

  .operator {
    background-color: #df8c5e;
  }

  .equals {
    background-color: #000;
    grid-column: 1 / span 2;
  }

  .one {
  }
  .two {
  }
  .three {
  }
  .plus {
  }
  .four {
  }
  .five {
  }
  .six {
  }
  .minus {
  }
  .seven {
  }
  .eight {
  }
  .nine {
  }
  .multiply {
  }
  .equals {
  }
  .zero {
  }
  .divide {
  }
}
</style>
