<template>
  <div class="calculator-wrapper">
    <div class="display">{{ expression }}</div>
    <button @click="clearExpression" class="clear">C</button>
    <button @click="toggleMinusPlus" class="toggle-plus-minus">+/-</button>
    <button @click="calculatePercent" class="percentage">%</button>
    <button @click="enterOperator" class="power operator">^</button>
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
    <button @click="calculateReversedPolish" class="equals">=</button>
    <button @click="enterChar" class="zero number">0</button>
    <button @click="enterChar" class="decimal">.</button>
    <button @click="enterOperator" class="divide operator">/</button>
    <button @click="enterOperator" class="operator">(</button>
    <button @click="enterOperator" class="operator">)</button>
    <button @click="calculateSqrt" class="operator">√</button>
    <button @click="goBack" class="back-button">&lt;</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      operators: [
        {
          char: '(',
          value: 0,
        },
        {
          char: ')',
          value: 0,
        },
        {
          char: '+',
          value: 1,
          calc(a, b) {
            return a + b
          },
        },
        {
          char: '-',
          value: 1,
          calc(a, b) {
            return a - b
          },
        },
        {
          char: '*',
          value: 2,
          calc(a, b) {
            return a * b
          },
        },
        {
          char: '/',
          value: 2,
          calc(a, b) {
            return a / b
          },
        },
        {
          char: '^',
          value: 3,
          calc(a, b) {
            return Math.pow(a, b)
          },
        },
        {
          char: '√',
          value: 3,
          calc(a) {
            return Math.sqrt(a)
          },
        },
      ],
      reversedPolish: [],
      stack: [],
      expression: '0',
    }
  },
  methods: {
    calculateSqrt() {
      let arr = this.expression.split(' ')

      arr[arr.length - 1] = Math.sqrt(+arr[arr.length - 1])
      this.expression = arr.join(' ')
    },
    clearExpression() {
      this.expression = '0'
    },
    convertToReversedPolish() {
      const charArray = this.expression.trim().split(' ')

      if (isNaN(+charArray[charArray.length - 1])) {
        charArray.pop()
      }

      charArray.forEach(char => {
        if (!isNaN(+char)) {
          this.reversedPolish.push(+char)
        } else {
          if (this.stack.length == 0 || char === '(') {
            this.stack.push(char)
          } else {
            let value = this.operators.find(op => op.char == char).value
            let lastElValue = this.operators.find(
              op => op.char == this.stack.slice(-1)
            ).value

            if (value > lastElValue) {
              this.stack.push(char)
            } else {
              while (value <= lastElValue && this.stack.length > 0) {
                if (lastElValue != 0) {
                  this.reversedPolish.push(this.stack.pop())
                } else {
                  this.stack.pop()
                  break
                }

                if (this.stack.length) {
                  lastElValue = this.operators.find(
                    op => op.char == this.stack.slice(-1)
                  ).value
                }
              }
              if (char != ')') {
                this.stack.push(char)
              }
            }
          }
        }
      })

      while (this.stack.length) {
        this.reversedPolish.push(this.stack.pop())
      }
    },
    calculateReversedPolish() {
      this.convertToReversedPolish()

      for (let i = 0; i < this.reversedPolish.length; i++) {
        if (this.reversedPolish[i] === '(' || this.reversedPolish[i] === ')') {
          this.reversedPolish.splice(i, 1)
        }
      }

      for (let i = 0; i < this.reversedPolish.length; i++) {
        if (isNaN(this.reversedPolish[i])) {
          let operator = this.operators.find(
            op => op.char == this.reversedPolish[i]
          )
          const sum = operator.calc(
            this.reversedPolish[i - 2],
            this.reversedPolish[i - 1]
          )
          this.reversedPolish.splice(i - 2, 3, sum)

          i -= 2
        }
      }

      this.expression = this.reversedPolish[0].toString()
      this.reversedPolish = []
    },
    calculatePercent() {
      let arr = this.expression.split(' ')

      if (arr[arr.length - 2] === '+' || arr[arr.length - 2] === '-') {
        arr[arr.length - 1] = arr[arr.length - 1] / arr[arr.length - 3]
      } else {
        arr[arr.length - 1] = arr[arr.length - 1] / 100
      }

      this.expression = arr.join(' ')
    },
    enterChar(e) {
      if (this.expression === '0' && e.target.innerText != '.') {
        this.expression = ''
      }
      if (this.expression.endsWith(' 0') && e.target.innerText != '.') {
        this.expression = this.expression.slice(0, -1)
      }
      this.expression += e.target.innerText
    },
    enterOperator(e) {
      if (e.target.innerText === '(') {
        this.expression += '' + e.target.innerText + ' '
      } else if (e.target.innerText === ')') {
        this.expression += ' ' + e.target.innerText
      } else if (this.expression.slice(-1) != ' ' && this.expression != '') {
        this.expression += ' ' + e.target.innerText + ' '
      }
    },
    goBack() {
      let arr = this.expression.split('')
      if (arr[arr.length - 1] == ' ') {
        arr.pop()
        arr.pop()
        arr.pop()
      } else {
        arr.pop()
      }

      this.expression = arr.join('')
    },
    toggleMinusPlus() {
      let arr = this.expression.split(' ')

      if (arr[arr.length - 1].startsWith('-')) {
        arr[arr.length - 1] = arr[arr.length - 1].slice(1)
      } else {
        arr[arr.length - 1] = `-${arr[arr.length - 1]}`
      }

      this.expression = arr.join(' ')
    },
  },
}
</script>

<style lang="scss" scoped>
.calculator-wrapper {
  background-color: #2e2727;
  display: grid;
  font-family: Arial, Helvetica, sans-serif;
  gap: 1.2rem;
  grid-template-columns: repeat (4, 1fr);
  grid-template-rows: repeat (7, 1fr);
  min-height: 58rem;
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
  }
}
</style>
