<template>
  <div>
    <div class="container" v-if="!showLoading">
      <div class="center-aligned">
        <h1>{{ randomTitle }}</h1>
        <h2>Твое предсказание на {{ currentDate }}</h2>
        <div class="content-box">
          <h3>{{ randomContent }}</h3>
        </div>
        <div class="button-group">
          <button @click="getNewPrediction">Получить еще предсказание</button>

        </div>
        <div class="button-group">
          <button @click="shareWithFriends">Поделиться</button>
          <button @click="sendToWall">Отправить на стену</button>
        </div>
      </div>
    </div>
    <LoadingAnimation v-if="showLoading" @loading-complete="updatePrediction" />
  </div>
</template>

<script>
import LoadingAnimation from './LoadingAnimation.vue';
import bridge from '@vkontakte/vk-bridge';


export default {
  name: 'DailyDetails',
  components: {
    LoadingAnimation
  },
  data() {
    return {
      currentDate: this.getFormattedDate(),
      randomTitle: '',
      randomContent: '',
      showLoading: false,
      lastAdTime: 0,
      titles: [
        "Загадочное предсказание",
        "Тайна будущего",
        "Волшебный прогноз",
        "Секретные знаки",
        "Магический сигнал",
        "Пророческий сон",
        "Тайна судьбы",
        "Невидимая сила",
        "Зов вселенной",
        "Загадочный символ",
        "Космическое послание",
        "Сказочное видение",
        "Волшебная подсказка",
        "Необычное совпадение",
        "Тайный шепот",
        "Мистическое предзнаменование",
        "Предвестие счастья",
        "Загадка грядущего",
        "Скрытый знак",
        "Небесный посыл",
        "Зов предков",
        "Магический намек",
        "Скрытое предсказание",
        "Тайное знамение",
        "Предзнаменование удачи",
        "Судьбоносное предсказание",
        "Загадочный намек",
        "Волшебная подсказка",
        "Тайный посыл",
        "Мистический знак",
        "Волшебная подсказка",
        "Тайный посыл",
        "Мистический знак",
        "Энергия космоса",
        "Звездный путь",
        "Незримый след",
        "Светлое будущее",
        "Загадочный поток",
        "Световая гармония",
        "Необычная встреча",
        "Звездное послание",
        "Тайные силы",
        "Лунный знак",
        "Космическая связь",
        "Пророческий свет",
        "Небесная гармония",
        "Вдохновляющее послание",
        "Загадка времени",
        "Секретное предсказание",
        "Тайные звезды",
        "Астрономический знак",
        "Тайные дары",
        "Звездная нить",
        "Космическая тайна",
        "Секретное послание",
        "Лунное предсказание",
        "Гармония вселенной",
        "Вдохновляющая загадка",
        "Тайное откровение",
        "Небесное предсказание",
        "Скрытые возможности",
        "Магическая нить"
      ],
      contents: [
        "Сегодня тебя ждет что-то удивительное.",
        "Будь готов к приятным сюрпризам.",
        "Возможно, скоро осуществится твоя мечта.",
        "Тебя ждет встреча с важным человеком.",
        "Неожиданное счастье придет в твою жизнь.",
        "Будущее приносит радостные новости.",
        "Ты найдешь то, что давно искал.",
        "Скоро откроется новая возможность.",
        "Судьба готовит для тебя приятный подарок.",
        "Ты встретишь старого друга.",
        "Возможна финансовая прибыль.",
        "Тебя ждет незабываемое приключение.",
        "Скоро ты получишь важное сообщение.",
        "Сегодня тебе улыбнется удача.",
        "Ты встретишь свою вторую половинку.",
        "В твою жизнь войдет гармония.",
        "Неожиданный гость принесет радость.",
        "Скоро ты достигнешь поставленной цели.",
        "Судьба будет к тебе благосклонна.",
        "Ты откроешь для себя что-то новое.",
        "Сегодня тебе будет сопутствовать успех.",
        "Будь готов к неожиданным переменам.",
        "Ты найдешь решение старой проблемы.",
        "Скоро ты получишь приятный сюрприз.",
        "В твою жизнь войдут новые люди.",
        "Тебя ждет успех в начинаниях.",
        "Сегодня тебе повезет.",
        "Ты встретишь важного человека.",
        "Скоро сбудется твоя мечта.",
        "Тебя ждет радостная новость.",
        "Твой день будет полон радости и смеха.",
        "Впереди ждут положительные перемены.",
        "Скоро тебя порадует приятное событие.",
        "Сегодня важно прислушиваться к интуиции.",
        "Твоя энергия привлекает удачу.",
        "Скоро ты получишь долгожданный ответ.",
        "Сегодня стоит рискнуть в новом начинании.",
        "В ближайшее время произойдут важные изменения.",
        "Ты получишь помощь от неожиданного источника.",
        "Ожидай новостей, которые изменят твою жизнь.",
        "Ты сможешь решить проблему, которая тебя беспокоила.",
        "Скоро в твоей жизни появится новый интересный проект.",
        "Важный выбор будет сделан к концу недели.",
        "Твоя преданность делу принесет плоды.",
        "Сегодня твои усилия будут вознаграждены.",
        "Будь открыт для новых возможностей.",
        "Тебе предстоит интересное путешествие.",
        "Скоро появится шанс улучшить свои финансовые дела.",
        "Ты получишь неожиданное предложение.",
        "Впереди радостные события и приятные сюрпризы.",
        "Сегодняшний день принесет особенное вдохновение.",
        "Ожидай поддержку от близких людей.",
        "Ты будешь успешен в своих начинаниях.",
        "Скоро ты узнаешь важную тайну.",
        "Новые знакомства окажутся весьма полезными.",
        "Скоро ты сможешь исправить старые ошибки.",
        "Сегодня твои мечты могут стать реальностью.",
        "Впереди множество приятных моментов.",
        "Тебе предстоит важное событие, которое изменит твою жизнь.",
        "Сегодняшний день отлично подходит для реализации твоих идей.",
        "В ближайшие дни твои желания начнут сбываться.",
        "Сегодня тебе стоит обратить внимание на детали.",
        "Скоро появится возможность сделать что-то важное.",
        "Твои старания начнут приносить результаты.",
        "Ты обнаружишь что-то ценное, что давно искал.",
        "Сегодня удача будет на твоей стороне.",
        "Ты можешь рассчитывать на поддержку близких.",
        "Новые начинания будут успешными и плодотворными.",
        "Скоро тебе удастся решить сложную задачу.",
        "Сегодняшний день принесет много позитивных эмоций.",
        "Ты получишь вдохновение для творческих проектов.",
        "Скоро твои усилия будут оценены по достоинству.",
        "Сегодня важно следовать своим убеждениям.",
        "Ты встретишь кого-то, кто окажет положительное влияние на твою жизнь.",
        "Будь готов к приятным неожиданностям в ближайшие дни.",
        "Твое упорство и трудолюбие приведут к успеху.",
        "Скоро ты получишь подтверждение своих догадок.",
        "Сегодня отличный день для завершения начатых дел.",
        "Ты сможешь найти решение старой проблемы.",
        "Тебя ждет встреча с человеком, который станет твоим союзником.",
        "Сегодня есть шанс на успешное завершение проекта.",
        "Будь готов к новым вызовам и возможностям.",
        "Твоя решительность принесет отличные результаты.",
        "Сегодняшние события помогут тебе продвинуться вперед.",
        "Ты сможешь сделать важный шаг на пути к своей цели.",
        "Скоро твоя жизнь наполнится радостью и гармонией.",
        "Ты откроешь для себя новые горизонты и возможности.",
        "Сегодня стоит быть открытым для изменений.",
        "Твое терпение и настойчивость будут вознаграждены.",
        "Ты можешь ожидать улучшения в личных отношениях.",
        "Сегодня подходящий день для планирования будущих целей.",
        "Тебя ждет позитивный поворот событий в ближайшее время.",
        "Скоро ты увидишь плоды своих усилий и работы.",
        "Твой день будет полон сюрпризов и приятных неожиданностей.",
        "Сегодня удачно сложатся все важные дела.",
        "Впереди благоприятный период для новых начинаний.",
        "Ты вскоре получишь долгожданную новость.",
        "Сегодня стоит довериться своему внутреннему голосу.",
        "Твоя позитивная энергия привлечет удачу.",
        "Скоро ты найдешь ключ к решению старой проблемы.",
        "Будь готов к радостным переменам в жизни.",
        "Сегодня отличный день для общения с близкими.",
        "Ты встретишь кого-то, кто вдохновит тебя на новые достижения.",
        "Твои планы начнут реализовываться быстрее, чем ожидалось.",
        "Скоро ты сможешь справиться с любыми трудностями.",
        "Сегодняшний день принесет тебе вдохновение для креативных идей.",
        "Ты сможешь справиться с любой сложной ситуацией.",
        "Скоро твои усилия будут признаны и оценены.",
        "Ты получишь возможность сделать что-то важное и полезное.",
        "Сегодняшний день будет полон приятных встреч и знакомств.",
        "Твоя решимость и настойчивость приведут к успеху.",
        "В ближайшее время ты сможешь осуществить свои желания.",
        "Ты получишь поддержку от неожиданного источника.",
        "Сегодня подходящий день для самосовершенствования.",
        "Ты сможешь найти решение для сложной задачи.",
        "Впереди приятные события и важные открытия.",
        "Ты обретешь уверенность в своих силах и возможностях.",
        "Сегодня стоит сосредоточиться на своих целях и планах.",
        "Скоро ты сможешь осуществить свои амбициозные идеи.",
        "Твои усилия начнут приносить ощутимые результаты.",
        "Будь готов к новым вызовам и возможностям, которые принесет день.",
        "Ты сможешь преодолеть преграды и добиться успеха.",
        "Сегодня будет удачный день для реализации своих проектов.",
        "Твое терпение и трудолюбие скоро будут вознаграждены.",
        "Скоро ты сможешь найти решение для финансовых вопросов.",
        "Сегодняшние события помогут тебе продвинуться вперед.",
        "Ты получишь возможность продемонстрировать свои таланты.",
        "Тебя ждет вдохновение и новые идеи для творчества.",
        "Сегодня важно верить в себя и свои силы.",
        "Скоро ты получишь долгожданное признание за свои достижения.",
        "Ты найдешь поддержку и понимание со стороны окружающих."
      ]
    };
  },
  methods: {
    getFormattedDate() {
      const options = { month: 'long', day: 'numeric' };
      return new Intl.DateTimeFormat('ru-RU', options).format(new Date());
    },
    shareWithFriends() {
      bridge.send('VKWebAppShare', {
        link: 'https://vk.com/app52059984'
      })
        .then((data) => {
          if (data.result) {
            // Запись размещена
          }
        })
        .catch((error) => {
          // Ошибка
          console.log(error);
        });
    },
    getRandomItem(array) {
      return array[Math.floor(Math.random() * array.length)];
    },
    getNewPrediction() {
      this.showLoading = true;

      const now = Date.now();
      if (now - this.lastAdTime < 30000) {
        //console.log('Wait 30 seconds before showing the ad again');
        
        return;
      }

      bridge.send('VKWebAppShowNativeAds', {
        ad_format: 'interstitial' /* Тип рекламы */
      })
        .then((data) => {
          if (data.result) {
            this.lastAdTime = Date.now(); // Update the last ad time
          } else {
            console.log('Ad error');
          }
        })
        .catch((error) => { console.log(error); });
      

    },
    sendToWall() {
      bridge.send('VKWebAppShowWallPostBox', {
        message: 'Мое предсказание на ' + this.currentDate + ': "' + this.randomContent + '"\nПолучи предсказание на сегодня!',
        attachment: 'https://vk.com/app52059984',
      })
        .then((data) => {
          // Запись отправлена на стену
          console.log(`Идентификатор записи: ${data.post_id}`);
        })
        .catch((e) => {
          console.log("Ошибка!", e);
        })
    },
    updatePrediction() {
      this.randomTitle = this.getRandomItem(this.titles);
      this.randomContent = this.getRandomItem(this.contents);
      this.showLoading = false;
    }
  },
  created() {
    this.randomTitle = this.getRandomItem(this.titles);
    this.randomContent = this.getRandomItem(this.contents);
  }
};
</script>

<style></style>
