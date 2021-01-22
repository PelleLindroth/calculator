<template>
  <div class="calculator-wrapper">
    <div class="display">{{ expression }}</div>
    <button @click="enterChar" class="one number">1</button>
    <button @click="enterChar" class="two number">2</button>
    <button @click="enterChar" class="three number">3</button>
    <button @click="enterOperator" class="plus operator">+</button>
    <button @click="enterChar" class="four number">4</button>
    <button @click="enterChar" class="five number">5</button>
    <button @click="enterChar" class="six number">6</button>
    <button @click="enterOperator" class="minus operator">-</button>
    <button @click="enterChar" class="seven number">7</button>
    <button @click="enterChar" class="eight number">8</button>
    <button @click="enterChar" class="nine number">9</button>
    <button @click="enterOperator" class="multiply operator">*</button>
    <button @click="calculate" class="equals">=</button>
    <button @click="enterChar" class="zero number">0</button>
    <button @click="enterOperator" class="divide operator">/</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      operators: [
        {
          char: '+',
          value: 1,
        },
        {
          char: '-',
          value: 1,
        },
        {
          char: '*',
          value: 2,
        },
        {
          char: '/',
          value: 2,
        },
        {
          char: '^',
          value: 3,
        },
      ],
      reversedPolish: [],
      stack: [],
      expression: '',
    }
  },
  methods: {
    calculate() {
      const charArray = this.expression.trim().split(' ')

      if (isNaN(+charArray[charArray.length - 1])) {
        charArray.pop()
      }

      charArray.forEach(char => {
        if (!isNaN(+char)) {
          this.reversedPolish.push(+char)
        } else {
          if (this.stack.length == 0) {
            this.stack.push(char)
          } else {
            console.log(this.stack[this.stack.length - 1])
            let value = this.operators.find(op => op.char == char).value
            let lastElValue = this.operators.find(
              op => op.char == this.stack.slice(-1)
            ).value

            if (value > lastElValue) {
              this.stack.push(char)
            } else {
              while (value <= lastElValue && this.stack.length > 0) {
                console.log(lastElValue)
                this.reversedPolish.push(this.stack.pop())

                if (this.stack.length > 1) {
                  lastElValue = this.operators.find(
                    op => op.char == this.stack.slice(-1)
                  ).value
                }
              }
              this.stack.push(char)
            }
          }
        }
      })

      while (this.stack.length > 0) {
        this.reversedPolish.push(this.stack.pop())
      }
      console.log(this.reversedPolish)
      console.log(this.stack)
      // 12 + 6 * 3 - 5

      // 12 6 3 * + 5 -
      // 12 18 + 5 -
      // 30 5 -
      // 25
    },
    enterChar(e) {
      if (this.expression.endsWith(' 0')) {
        this.expression = this.expression.slice(0, -1)
      }
      this.expression += e.target.innerText
    },
    enterOperator(e) {
      if (this.expression.slice(-1) != ' ' && this.expression != '') {
        this.expression += ' ' + e.target.innerText + ' '
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.calculator-wrapper {
  background-color: #2e2727;
  display: grid;
  font-family: Arial, Helvetica, sans-serif;
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
    display: grid;
    grid-column: 1 / span 4;
    grid-row: 1;
    line-height: 1;
    min-height: 3.1rem;
    padding: 0 0.5rem;
    place-items: center end;
  }

  .number,
  .operator,
  .equals {
    border-radius: 0.5rem;
    border: none;
    color: white;
    display: grid;
    font-size: 3rem;
    line-height: 1;
    outline: none;
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

  // .one {
  // }
  // .two {
  // }
  // .three {
  // }
  // .plus {
  // }
  // .four {
  // }
  // .five {
  // }
  // .six {
  // }
  // .minus {
  // }
  // .seven {
  // }
  // .eight {
  // }
  // .nine {
  // }
  // .multiply {
  // }
  // .equals {
  // }
  // .zero {
  // }
  // .buttonide {
  // }
}
</style>
