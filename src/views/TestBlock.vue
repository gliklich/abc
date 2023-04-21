<template>
  <div class="test__bg">
    <img class="pass__bg" src="../assets/img/rain_bk2.png" alt="background image" />
    <div class="test-container">
      <div class="progress-bar-container" v-if="!completed">
        <div class="progress-bar" :style="{ width: progress + '%' }"></div>
      </div>
      <div class="question-container" v-if="!completed">
        <div class="question-type" v-if="questionType === 'question'">
          <h2 class="question__text">{{ currentQuestion.question }}</h2>
          <ul class="test__list">
            <li class="test__item" v-for="(option, index) in currentQuestion.options" :key="index">
              <div class="test__custom-radio">
                <input
                  class="test__input"
                  type="radio"
                  :id="'option' + index"
                  :value="option"
                  v-model="selectedOption"
                />
                <label class="test__label" :for="'option' + index">{{ option }}</label>
              </div>
            </li>
          </ul>
          <button class="test__button button" @click="nextQuestion" :disabled="!selectedOption">
            Далее
          </button>
        </div>
        <div class="question-type" v-else-if="questionType === 'color'">
          <h2 class="question__text">{{ currentQuestion.question }}</h2>
          <ul class="test__list_color">
            <li
              class="test__item_color"
              v-for="(option, index) in currentQuestion.options"
              :key="index"
            >
              <div class="test__custom-radio_color">
                <input
                  class="test__input_color"
                  type="radio"
                  :id="'option' + index"
                  :value="option"
                  v-model="selectedOption"
                />
                <label
                  class="test__label_color"
                  :for="'option' + index"
                  :style="{ backgroundColor: option }"
                ></label>
              </div>
            </li>
          </ul>
          <button class="test__button button" @click="nextQuestion" :disabled="!selectedOption">
            Далее
          </button>
        </div>
        <div class="question-type" v-else-if="questionType === 'pictureInPicture'">
          <h2 class="question__text">{{ currentQuestion.question }}</h2>
          <img :src="currentQuestion.picture" alt="Question" />
          <ul class="test__list_pInP">
            <li
              class="test__item_pInP"
              v-for="(option, index) in currentQuestion.options"
              :key="index"
            >
              <div class="test__custom-radio_pInP">
                <input
                  class="test__input_pInP"
                  type="radio"
                  :id="'option' + index"
                  :value="option"
                  v-model="selectedOption"
                />
                <label class="test__label_pInP" :for="'option' + index"
                  ><span>{{ option }}</span></label
                >
              </div>
            </li>
          </ul>
          <button class="test__button button" @click="nextQuestion" :disabled="!selectedOption">
            Далее
          </button>
        </div>
        <div class="question-type" v-else-if="questionType === 'questionAndPicture'">
          <h2 class="question__text">{{ currentQuestion.question }}</h2>
          <img :src="currentQuestion.picture" alt="Question" />
          <ul class="test__list">
            <li class="test__item" v-for="(option, index) in currentQuestion.options" :key="index">
              <div class="test__custom-radio">
                <input
                  class="test__input"
                  type="radio"
                  :id="'option' + index"
                  :value="option"
                  v-model="selectedOption"
                />
                <label class="test__label" :for="'option' + index">{{ option }}</label>
              </div>
            </li>
          </ul>
          <button class="test__button button" @click="nextQuestion" :disabled="!selectedOption">
            Далее
          </button>
        </div>
      </div>
      <div class="question-test__processing" v-else-if="load">
        <h1 class="question-test__title">Обработка результатов</h1>
        <div class="question-test__loader">
          <img src="../assets/img/loader.svg" alt="loader" />
        </div>
        <div class="question-test__text">
          <span>Определение стиля мышления...............</span
          >...............................................................
        </div>
      </div>
      <div class="question-test__complete complete-test" v-else>
        <div class="complete-test__container">
          <div class="complete-test__title">Ваш результат рассчитан:</div>
          <div class="complete-test__text">
            Вы относитесь к 3% респондентов, чей уровень интеллекта более чем на 15 пунктов
            отличается от среднего в большую или меньшую сторону!
          </div>
          <div class="complete-test__label">Скорее получите свой результат!</div>
          <div class="complete-test__textBlock">
            В целях защиты персональных данных результат теста, их подробная интерпретация и
            рекомендации доступны в виде голосового сообщения по звонку с вашего мобильного телефона
          </div>
          <PhoneTimer @timer-finished="onTimerFinished" />
          <button
            @submit.prevent
            class="complete-test__phone"
            @click="makeRequest"
            :disabled="timerFinished"
          >
            <div class="complete-test__icon"><img src="../assets/img/phone.svg" alt="phone" /></div>
            <div class="complete-test__link">Позвонить и прослушать результат</div>
          </button>
          <div v-if="response" class="complete-test__result">
            <p>Имя: {{ response.name }}</p>
            <p>Рост: {{ response.height }}</p>
            <p>Вес: {{ response.mass }}</p>
            <p>Цвет волос: {{ response.hair_color }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import PhoneTimer from '../components/UI/PhoneTimer.vue'
export default {
  components: {
    PhoneTimer
  },
  setup() {
    const progress = ref(0)
    let timerId = null

    const startTimer = () => {
      progress.value = 0
      clearTimeout(timerId)
      let startTime = Date.now()
      const endTime = startTime + 2 * 60 * 1000 // 2 minutes in milliseconds
      const updateProgress = () => {
        const currentTime = Date.now()
        const remainingTime = endTime - currentTime
        progress.value = Math.max(0, Math.min(100, (remainingTime / (2 * 60 * 1000)) * 100))
        if (currentTime < endTime) {
          timerId = setTimeout(updateProgress, 1000)
        }
      }
      updateProgress()
    }

    onMounted(startTimer)

    return {
      progress,
      startTimer
    }
  },

  data() {
    return {
      questions: [
        {
          id: '1',
          questionType: 'question',
          question: 'Ваш пол:',
          options: ['Мужчина', 'Женщина'],
          answer: 'Мужчина'
        },
        {
          id: '2',
          questionType: 'question',
          question: 'Укажите ваш возраст:',
          options: ['До 18', 'От 18 до 28', 'От 29 до 35', 'От 36'],
          answer: 'От 29 до 35'
        },
        {
          id: '3',
          questionType: 'question',
          question: 'Выберите лишнее:',
          options: ['Дом', 'Шалаш', 'Бунгало', 'Скамейка', 'Хижина'],
          answer: 'Скамейка'
        },
        {
          id: '4',
          questionType: 'question',
          question: 'Выберите лишнее:',
          options: ['Хижина', 'Скамейка', 'Дом', 'Шалаш', 'Бунгало'],
          answer: 'Скамейка'
        },
        {
          id: '5',
          questionType: 'question',
          question: 'Продолжите числовой ряд: 18 20 24 32',
          options: ['62', '48', '74', '57', '60', '77'],
          answer: '48'
        },
        {
          id: '6',
          questionType: 'question',
          question: 'Какой из городов лишний?',
          options: ['Вашингтон', 'Лондон', 'Париж', 'Нью-Йорк', 'Москва', 'Оттава'],
          answer: 'Нью-Йорк'
        },
        {
          id: '7',
          questionType: 'question',
          question: 'Вам привычнее и важнее:',
          options: [
            'Наслаждаться каждой минутой проведенного времени',
            'Быть устремленными мыслями в будущее',
            'Учитывать в ежедневной практике прошлый опыт'
          ],
          answer: 'Нью-Йорк'
        },
        {
          id: '8',
          questionType: 'color',
          question: 'Выберите цвет, который сейчас наиболее Вам приятен:',
          options: [
            '#A8A8A8',
            '#0000A9',
            '#00A701',
            '#F60100',
            '#FDFF19',
            '#A95403',
            '#000000',
            '#850068',
            '#46B3AC'
          ],
          answer: '#0000A9'
        },
        {
          id: '9',
          questionType: 'color',
          question:
            'Отдохните пару секунд, еще раз Выберите цвет, который сейчас наиболее Вам приятен:',
          options: [
            '#A8A8A8',
            '#46B2AC',
            '#A95403',
            '#00A701',
            '#000000',
            '#F60100',
            '#850068',
            '#FDFF19',
            '#0000A9'
          ],
          answer: '#00A701'
        },
        {
          id: '10',
          questionType: 'pictureInPicture',
          question: 'Выберите правильную фигуру из четырёх пронумерованных.',
          picture: '/src/assets/img/q10.png',
          options: ['1', '2', '3', '4'],
          answer: '2'
        },
        {
          id: '11',
          questionType: 'pictureInPicture',
          question: 'Вставьте подходящее число',
          picture: '/src/assets/img/q11.png',
          options: ['34', '36', '53', '44', '66', '42'],
          answer: '44'
        },
        {
          id: '12',
          questionType: 'questionAndPicture',
          question:
            'Какое определение, по-Вашему, больше подходит к этому геометрическому изображению:',
          picture: '/src/assets/img/q12.png',
          options: ['Оно остроконечное', 'Оно устойчиво', 'Оно-находится в состоянии равновесия'],
          answer: 'Оно остроконечное'
        }
      ],
      currentQuestionIndex: 0,
      selectedOption: null,
      correctAnswers: 0,
      load: true,
      completed: false,
      response: null,
      timerFinished: false
    }
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex]
    },
    questionType() {
      return this.currentQuestion.questionType
    }
  },
  methods: {
    onTimerFinished() {
      this.timerFinished = true
    },

    nextQuestion() {
      this.startTimer()

      // Проверяем, что пользователь выбрал вариант ответа
      if (this.selectedOption === null) {
        alert('Выберите вариант ответа!')
        return
      }
      // Проверяем, правильный ли выбран ответ
      if (this.selectedOption === this.currentQuestion.answer) {
        this.correctAnswers++
      }

      // Увеличиваем индекс текущего вопроса
      this.currentQuestionIndex++

      // Если прошли все вопросы, завершаем тест
      if (this.currentQuestionIndex === this.questions.length) {
        this.completed = true
        setInterval(() => {
          this.load = false
        }, 5000)
      }

      // Сбрасываем выбранный вариант ответа
      this.selectedOption = null
    },
    makeRequest() {
      axios
        .get('https://swapi.dev/api/people/1/')
        .then((response) => {
          this.response = response.data
        })
        .catch((error) => {
          console.error(error)
        })
    }
  }
}
</script>

<style scoped>
.progress-bar-container {
  width: 260px;
  height: 11px;
  background-color: #f2f3f37e;
  border-radius: 10.5px;
  margin-top: 17px;
}

.progress-bar {
  height: 100%;
  background-color: #3bde7c;
  transition: width 0.1s ease-out;
  border-radius: 10.5px;
}
.test__bg {
  position: relative;

  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.test-container {
  padding-top: 46px;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 100vh;
  position: relative;
  z-index: 3;
}

.question-container {
  display: flex;
  flex: 1 1 auto;
  width: 100%;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  margin-bottom: 20px;
}
.question-type {
  display: flex;
  flex: 1 1 auto;
  width: 100%;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  margin-bottom: 20px;
}
.question__text {
  font-size: 20px;
  line-height: 26px;
  text-align: center;
  letter-spacing: 0.05em;
  color: #fff;
  width: 280px;
}
.test__list {
  display: flex;
  flex-direction: column;
  gap: 8px;
  width: 100%;
}
.test__label {
  display: flex;
  padding: 16px 0 16px 84px;
  background-color: #2727276c;
  cursor: pointer;
  position: relative;
}

.test__label {
  /* добавляем стили для контейнера лейбла */
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  cursor: pointer;
}

/* скрываем оригинальный input */
.test__input {
  display: none;
}

/* создаем стилизованный label */
.test__label {
  display: inline-block;
  position: relative;
  padding-left: 30px;
  margin-right: 10px;
  font-size: 16px;
  line-height: 24px;
  cursor: pointer;
  width: 100%;
  padding: 16px 0px 16px 84px;
}

/* добавляем псевдоэлемент для имитации radio button */
.test__label:before {
  content: '';
  display: inline-block;
  position: absolute;
  left: 34px;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  border: 1px solid #ffffff;
  border-radius: 50%;
  background-color: transparent;
}

/* стилизуем псевдоэлемент при выбранном input */
.test__input:checked + .test__label:before {
  background-color: #000;
}

/* добавляем псевдоэлемент для имитации точки внутри radio button */
.test__label:after {
  content: '';
  display: inline-block;
  position: absolute;
  left: 34px;
  top: 50%;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 1px solid #272727;
  background-color: #2950c2;
  opacity: 0;
  transition: opacity 0.2s ease-in-out;
}

/* стилизуем псевдоэлемент при выбранном input */
.test__input:checked + .test__label:after {
  opacity: 1;
}
.test__input:checked + .test__label {
  background-color: #ffc700;
}
h3 {
  margin-bottom: 10px;
}

/* Color Type Question */
.test__list_color {
  display: flex;
  row-gap: 21px;
  column-gap: 24px;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  padding-top: 25px;
  padding-bottom: 34px;
}
.test__input_color {
  display: none;
}
.test__label_color {
  display: flex;
  width: 100%;
  height: 100%;
}
.test__custom-radio_color {
  width: 100%;
  height: 100%;
}
.test__item_color {
  width: 75px;
  height: 75px;
}
.test__label_color:after {
  content: '';
  display: inline-block;
  width: 81px;
  height: 81px;
  border: 6px solid #ffc700;
  opacity: 0;
  transition: opacity 0.2s ease-in-out;
}
.test__input_color:checked + .test__label_color:after {
  opacity: 1;
}

/* Picture in picture Type Question */

.test__list_pInP {
  display: flex;
  gap: 8px;
  width: 320px;

  /* flex-wrap: wrap; */
  align-items: center;
  justify-content: space-around;

  padding: 25px 17px 34px 17px;
}
.test__input_pInP {
  display: none;
}
.test__label_pInP {
}
.test__label_pInP span {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: #0d0c11;
  font-family: 'PT Serif';
  font-weight: 700;
  font-size: 20px;
  line-height: 49px;
  text-align: center;
}
.test__custom-radio_pInP {
  position: relative;
  width: 41px;
  height: 41px;
}
.test__item_pInP {
  width: 41px;
  height: 41px;
  background-color: #fff;
}
.test__label_pInP:after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  display: inline-block;
  width: 41px;
  height: 41px;
  border: 6px solid #ffc700;
  opacity: 0;
  transition: opacity 0.2s ease-in-out;
}
.test__input_pInP:checked + .test__label_pInP:after {
  opacity: 1;
}
.question-test__processing {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 280px;
}
.question-test__title {
  width: 150px;
  font-size: 23px;
  line-height: 30px;
  text-align: center;
  letter-spacing: 0.05em;
  color: #fff;
  padding-top: 50px;
  padding-bottom: 30px;
}

.question-test__text {
  max-width: 280px;
  font-size: 14px;
  line-height: 19px;
  letter-spacing: 0.05em;
  color: #fff;
}
.question-test__text span {
  /* white-space: nowrap; */
  display: flex;
}

.question-test__loader {
  margin-bottom: 28px;
}
.question-test__loader img {
  animation: spin 0.8s ease-in-out infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/*  Test complete  */
.question-test__complete {
}
.complete-test {
}
.complete-test__container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.complete-test__title {
  margin-top: 35px;
  margin-bottom: 15px;
  font-weight: 700;
  font-size: 15px;
  line-height: 16px;
  text-align: center;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #ffffff;
}
.complete-test__text {
  margin: 0 25px 25px 25px;
  font-size: 14px;
  line-height: 18px;
  text-align: center;
  text-decoration-line: underline;
}
.complete-test__label {
  font-weight: 700;
  font-size: 18px;
  line-height: 22px;
  text-align: center;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #3bde7c;
  margin-bottom: 15px;
}
.complete-test__textBlock {
  width: 256px;
  background-color: #1c2741;
  border-radius: 6px;
  padding: 12px 15px;
  font-weight: 500;
  font-size: 8px;
  line-height: 14px;
  text-align: center;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: #ffffff;
}

.complete-test__phone {
  display: flex;
  align-items: center;
  gap: 20px;
  width: 290px;
  background-color: #eb1b00;
  border-radius: 5px;
  padding: 25px 22px 25px 15px;
  cursor: pointer;
  transition: background-color 0.3s ease 0s;
}
.complete-test__phone:hover {
  background-color: #5f0b00;
}
.complete-test__icon {
}
.complete-test__result {
  margin: 15px 0;
  display: flex;
  flex-direction: column;
  gap: 5px;
}
</style>
