<script lang='ts' setup>
import { onMounted, ref } from 'vue';

const audio = ref([])
const numbers = ref<number[]>([])
const operation = ref(["OpPlus", "OpMinus", "OpMultiply"])
const equation = ref('')
const currentAudioIndex = ref(0)
const userInput = ref('')
const visbleEquation = ref(false)
const visibleRepeat = ref(false)
const blockButtonStart = ref(false)


//тоже самое, что и функция (только новый формат)
// const plus = () => {
//     1+1
// return
// }

const getRandomNumber = (min: number, max: number, count: number = 1) => {
    let randomNum: number[] = []
    for (let i: number = 0; i < count; i++) {
        randomNum.push(Math.trunc(Math.random() * (max - min) + min))
    }
    return randomNum
}

const shuffle = (array: string[]) => {
    array.sort(() => Math.random() - 0.5)
}

const startListen = () => {
    const element = new Audio(audio.value[currentAudioIndex.value]);
    element.play();
    currentAudioIndex.value++;
    blockButtonStart.value = true
    if (currentAudioIndex.value < audio.value.length) {
        setTimeout(startListen, 1200); // Задержка в 1 секунду перед проигрыванием следующего аудио файла
    } else {
        currentAudioIndex.value = 0
        blockButtonStart.value = false
    }
}

const addAudio = () => {
    numbers.value.forEach((el, i) => {
        if (el > 20 && el % 10 !== 0) {
            audio.value.push(`/NumbersMp3/${Math.trunc(el / 10) * 10}.mp3`)
            audio.value.push(`/NumbersMp3/${el % 10}.mp3`)

        } else {
            audio.value.push(`/NumbersMp3/${el}.mp3`)
        }

        equation.value += el

        if (i === 3) {
            audio.value.push(`/NumbersMp3/Answer${(i % 2) + 1}.mp3`)
            // equation.value += '='

        } else {
            audio.value.push(`/NumbersMp3/${operation.value[i]}${(i % 2) + 1}.mp3`)
            switch (operation.value[i]) {
                case "OpPlus":
                    equation.value += "+"
                    break
                case "OpMinus":
                    equation.value += "-"
                    break
                case "OpMultiply":
                    equation.value += "x"
                    break
            }
        }
    })
}

const addInputValue = (key: string) => {
    userInput.value += key
}

const deleteInputValue = () => {
    userInput.value = userInput.value.slice(0, -1)
}

const compareInput = () => {
    if (userInput.value.length >= 7) {
        visbleEquation.value = !visbleEquation.value
        visibleRepeat.value = !visibleRepeat.value
    }
}

const startTheGame = () => {
    visbleEquation.value = false
    visibleRepeat.value = false
    audio.value = []
    
    userInput.value = ''
    equation.value = ''
    numbers.value = getRandomNumber(0, 99, 4)
    shuffle(operation.value)
    addAudio()
}

onMounted(() => {
    startTheGame()

})


// синтаксический сахар - Егорчик Стайл!


</script>

<template>
    <h1>Включи звук и нажми старт, когда готов</h1>
    <h2 :class="{ 'block': blockButtonStart }" @click="startListen" v-if="!visibleRepeat">старт</h2>
    <h2 @click="startListen" v-else>повтор</h2>
    <h2 @click="startTheGame()" v-if="visbleEquation">хочу ещё уравнение</h2>


    <div class="calc">
        <div class="display" v-if="visbleEquation">
            {{ equation }}
        </div>
        <div class="display">
            {{ userInput }}
        </div>

        <div class="keypad">

            <div class="key num" @click="addInputValue('7')">7</div>
            <div class="key num" @click="addInputValue('8')">8</div>
            <div class="key num" @click="addInputValue('9')">9</div>
            <div class="key fn" @click="addInputValue('+')">+</div>

            <div class="key num" @click="addInputValue('4')">4</div>
            <div class="key num" @click="addInputValue('5')">5</div>
            <div class="key num" @click="addInputValue('6')">6</div>
            <div class="key fn" @click="addInputValue('-')">-</div>

            <div class="key num" @click="addInputValue('1')">1</div>
            <div class="key num" @click="addInputValue('2')">2</div>
            <div class="key num" @click="addInputValue('3')">3</div>
            <div class="key fn" @click="addInputValue('x')">x</div>

            <div class="key special" @click="deleteInputValue">🔙</div>
            <div class="key num" @click="addInputValue('0')">0</div>
            <div class="key fn" @click="addInputValue('/')">/</div>
            <div class="key fn" @click="compareInput">ok</div>

        </div>
    </div>
</template>

<style>
.block {
    opacity: .4;
    pointer-events: none;
}

.calc {
    width: 320px;
    height: 480px;
    display: flex;
    flex-direction: column;
    margin-left: auto;
    margin-right: auto;
    background-color: #D9D3C7;
    border: 2px solid #D9D3C7;
}

.display {
    flex: 1;
    background-color: #A5B3A6;
    margin: 10px;
    font-size: 40px;
    text-align: right;
    overflow-wrap: break-word;
    padding: 5px;
}

.keypad {
    height: 320px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    margin: 10px;
}

.key {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 40px;
    cursor: pointer;
}

.num {
    background-color: #525759;
    color: #ffffff;
}

.fn {
    background-color: #877569;
    color: #000000;
}

.special {
    background-color: #BD5A04;
    color: #000000;
    font-size: 35px;
    font-weight: bold;
}

::selection {
    background: none;
}
</style>