<template lang="pug">
  .calculator
    .wrapper
      .title
        h1 Calculadora Vue
      .calculatorScreen
        input(type="text" placeholder= "0" v-model="previousOperation" disabled).previousOperation
        input(type="text" placeholder= "0" v-model="operation" disabled)
      .buttons
        .first-row
          button.sign(@click="resetBtn") AC
          button.sign(@click="signsBtn('%')") %
          button.sign(@click="deleteLast")
            i.fas.fa-delete-left
          button.sign(@click="signsBtn('/')") /
        .second-row
          button( @click="numberBtn('7')") 7
          button( @click="numberBtn('8')") 8
          button( @click="numberBtn('9')") 9
          button.sign(@click="signsBtn('*')") *
        .second-row
          button( @click="numberBtn('4')") 4
          button( @click="numberBtn('5')") 5
          button( @click="numberBtn('6')") 6
          button.sign(@click="signsBtn('-')") -
        .third-row
          button( @click="numberBtn('1')") 1
          button( @click="numberBtn('2')") 2
          button( @click="numberBtn('3')") 3
          button.sign(@click="signsBtn('+')") +
        .fourth-row
          button( @click="numberBtn('0')") 0
          button( @click="numberBtn('.')") .
          button.equal(@click="calculate(sign)") =
</template>

<script setup>
import '@fortawesome/fontawesome-free/css/all.css'
import { ref } from 'vue'

const operation = ref('')
const previousOperation = ref('')

const deleteLast = () => {
  if (operation.value === 'No se puede dividir entre 0') operation.value = ''
  operation.value = operation.value.slice(0, -1)
  if (operation.value.endsWith(" ")) {
    operation.value = operation.value.slice(0, -1)
  }
};
const resetBtn = () => {
  operation.value = ''
  previousOperation.value = ''
};

const numberBtn = (digit) => {
  if (operation.value === 'No se puede dividir entre 0') operation.value = ''
  if (digit === '.') {
    const array = operation.value.split(' ')
    if (array[array.length - 1].includes('.')) return
    
  }
  operation.value += digit
}

const signsBtn = (operator) => {
  if (operation.value === 'No se puede dividir entre 0') operation.value = ''
  const regex = /[\D]+/g;
  const sign = operation.value.match(regex)
  if (sign) {
    if (operation.value.charAt(operation.value.length - 2) === '%') {
      if (operator === '+' || operator === '-' || operator === '*' || operator === '/' || operator === '%') {
        return
      }
    } else if (operation.value.charAt(operation.value.length - 2) === '*' || operation.value.charAt(operation.value.length - 2) === '/') {
      if (operator === '+') {
        return
      } else if (operator === '*' || operator === '/' || operator === '%') {
        return
      } else if (operator === '-') {
        return operation.value += operator
      }
    } else {
      calculate(sign)
    }
  }
  operation.value += ` ${operator} `
}

const calculate = () => {
  previousOperation.value = operation.value;
  const operators = operation.value.split(' ');

  switch (operators[1]) {
    case "+":
      operation.value = Number(operators[0]) + Number(operators[2])
      break;
    case "-":
      operation.value = Number(operators[0]) - Number(operators[2])
      break;
    case "*":
      operation.value = Number(operators[0]) * Number(operators[2])
      break;
    case "/":
      if (Number(operators[2]) === 0) {
        operation.value = 'No se puede dividir entre 0'
        break
      }
      operation.value = Number(operators[0]) / Number(operators[2])
      break;
    case "%":
      operation.value = Number(operators[0]) / 100 * Number(operators[2])
    default:
      console.log('pendiente por calcular este simbolo ' +  operators);

  }

  operation.value = operation.value.toString()
}
</script>

<style scoped lang="scss">
.calculator {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: 'Arial', sans-serif;
  height: 100vh;

  .wrapper {
    width: 300px;
    background-color: rgb(56, 56, 56);
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    padding: 20px;

    .title {
      text-align: center;
      color: #cb6900;
      margin-bottom: 20px;

      h1 {
        font-size: 1.5em;
        font-weight: bold;
      }
    }

    .calculatorScreen {
      width: 100%;
      height: 70px;
      background-color: #fff;
      border-radius: 5px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin-bottom: 20px;

      input {
        width: 90%;
        height: 30px;
        border: none;
        text-align: right;
        font-size: 1.2em;
        color: #333;
        background-color: transparent;
      }

      .previousOperation {
        color: rgb(111, 111, 111);
      }
    }

    .buttons {
      gap: 10px;
      display: grid;
      justify-content: center;
      text-align: center;

      button {
        width: 60px;
        height: 60px;
        margin: 5px;
        font-size: 1.5em;
        border: none;
        border-radius: 50px;
        background-color: #e0e0e0;
        cursor: pointer;
        transition: background-color 0.3s;

        &:hover {
          background-color: #d0d0d0;
        }

        i {
          font-size: 1.2em;
        }
      }

      .sign {
        background-color: #cb6900;
        color: white;

        &:hover {
          background-color: #b85a00;
        }
      }

      .equal {
        background-color: #4caf50;
        color: white;

        &:hover {
          background-color: #45a049;
        }
      }
    }
  }
}
</style>
