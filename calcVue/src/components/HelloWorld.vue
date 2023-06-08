<template>
  <div class="w-[100vw] h-[100vh] flex justify-center">
    <div id="disp" class="w-[50vw] h-[90vh] border-4 rounded-2xl shadow-2xl">
      <header class="flex flex-col mt-3 mx-4">
        <div class="flex justify-between">
          <time>{{ hours }}:{{ min }} </time>

          <div class="flex">
            <i class="fa mx-2">&#xf1eb;</i>
            <i class="fa mx-2">&#xf240;</i>
            <i class="fa mx-2">&#xf012;</i>
          </div>
        </div>
        <div class="flex mt-3 justify-between items-center">
          <i class="fa text-[20px]">&#xf039;</i>
          <button id="calcBtn" class="border rounded-xl bg-teal-500 p-3">
            <p id="calcLabel" class="text-white">Calculator</p>
          </button>
          <p id="convLabel">Converter</p>
          <button>
            <i class="fa text-[20px]" @click="toggleTheme()">&#xf186;</i>
          </button>
        </div>
      </header>
      <main class="flex flex-col">
        <div class="flex flex-col w-full h-[200px] justify-center text-4xl">
          <div v-if="calculate" class="text-lg ml-[100px]">
            <p id="result">{{ historyData[historyData.length - 1] }}</p>
          </div>
          <div class="flex gap-x-2 mt-5 justify-center">
            <div v-if="operation" class="">
              <p id="result">{{ prevNumber }}</p>
            </div>
            <div v-if="operation" class="">
              <p id="result">{{ selectedOperation }}</p>
            </div>
            <div class="">
              <p id="result">{{ currentNumber }}</p>
            </div>
            <div v-if="calculate" class="flex gap-2">
              <p id="result" v-if="total">=</p>
              <p id="result">{{ total }}</p>
            </div>
          </div>
        </div>

        <div class="flex flex-col items-center">
          <div class="flex w-[100%] justify-center">
            <div class="grid grid-cols-4 gap-x-20 gap-y-10 text-4xl">
              <button class="" @click="clear('c')">AC</button>
              <button class="fa" @click="operation('()')">&#xf057;</button>
              <button class="" @click="operation('%')">&#37;</button>
              <button @click="operation('/')">&#247;</button>

              <button class="" @click="output('7')">7</button>
              <button class="" @click="output('8')">8</button>
              <button class="" @click="output('9')">9</button>
              <button class="" @click="operation('*')">&#215;</button>

              <button class="" @click="output('4')">4</button>
              <button class="" @click="output('5')">5</button>
              <button class="" @click="output('6')">6</button>
              <button class="" @click="operation('-')">&#8722;</button>

              <button class="" @click="output('1')">1</button>
              <button class="" @click="output('2')">2</button>
              <button class="" @click="output('3')">3</button>
              <button class="" @click="operation('+')">&#43;</button>

              <button class="" @click="output('()')">&#177;</button>
              <button class="" @click="output('0')">0</button>
              <button class="" @click="output('.')">&#8231;</button>
              <button class="" @click="calculate('=')">&#61;</button>
            </div>
          </div>
        </div>
      </main>
      <footer></footer>
    </div>
  </div>
</template>

<script>
import { onMounted, ref } from "vue";

export default {
  setup() {
    let date = new Date();
    let hours = date.getHours();
    if (hours < 10) {
      hours = "0" + hours;
    }
    let min = date.getMinutes();
    if (min < 10) {
      min = "0" + min;
    }

    // const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"];
    // const operators = ["*", "/", "+", "-", "%", "()"];

    const currentNumber = ref("");
    const prevNumber = ref("");
    const selectedOperation = ref("");
    const total = ref("");
    let historyData = [];

    const output = (value) => {
      currentNumber.value = currentNumber.value + value;
    };

    const calculate = () => {
      const multiply = () => {
        total.value = currentNumber.value * prevNumber.value;
      };

      const divide = () => {
        total.value = prevNumber.value / currentNumber.value;
      };

      const sum = () => {
        total.value = +prevNumber.value + +currentNumber.value;
      };

      const substr = () => {
        total.value = prevNumber.value - currentNumber.value;
      };

      const percent = () => {
        total.value = currentNumber.value / 100;
      };

      const sign = () => {};

      if (selectedOperation.value === "*") multiply();
      else if (selectedOperation.value === "/") divide();
      else if (selectedOperation.value === "+") sum();
      else if (selectedOperation.value === "-") substr();
      else if (selectedOperation.value === "()") sign();
      else if (selectedOperation.value === "%") percent();

      historyData.push(`${total.value}`);

      console.log(historyData);
    };

    const clear = () => {
      total.value =
        prevNumber.value =
        selectedOperation.value =
        currentNumber.value =
          "";
    };

    const operation = (value) => {
      selectedOperation.value = value;
      prevNumber.value = currentNumber.value;
      currentNumber.value = "";
    };

    const switchDisp = (themeName) => {
      localStorage.setItem("theme", themeName);
      document.documentElement.className = themeName;
    };

    const toggleTheme = () => {
      if (localStorage.getItem("theme") === "theme-dark") {
        switchDisp("theme-light");
      } else {
        switchDisp("theme-dark");
      }
    };
    (function () {
      if (localStorage.getItem("theme") === "theme-dark") {
        switchDisp("theme-dark");
      } else {
        switchDisp("theme-light");
      }
    })();

    return {
      historyData,
      hours,
      min,
      output,
      calculate,
      operation,
      clear,
      currentNumber,
      prevNumber,
      total,
      selectedOperation,
      switchDisp,
      toggleTheme,
    };
  },
};
</script>

<style>
.grid button:hover {
  color: rgba(127, 255, 212, 0.979);
}

.theme-dark {
  --color-primary: #17ed90;
  --color-secondary: #334155;
  --color-accent: rgb(20 184 166);
  --font-color: #ffffff;
}

.theme-light {
  --color-primary: #0060df;
  --color-secondary: #fbfbfe;
  --color-accent: #fd6f53;
  --font-color: #000000;
}

#disp {
  background-color: var(--color-secondary);
}

/* ======DARK THEME======= */
.theme-dark #result {
  color: #fbfbfe;
}

.theme-dark button {
  color: #fbfbfe;
}

.theme-dark time {
  color: #fbfbfe;
}

.theme-dark i {
  color: #fbfbfe;
}

.theme-dark #calcBtn {
  background: #fbfbfe;
}
.theme-dark #calcBtn:hover {
  background: #cbd5e1;
}
.theme-dark #calcLabel {
  color: var(--color-accent);
}
.theme-dark #convLabel {
  color: whitesmoke;
}

/* ======LIGHT THEME======= */

.theme-light button,
p,
i,
time {
  color: black;
}
</style>
