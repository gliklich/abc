<template>
  <main class="page">
    <section class="page__pass pass">
      <img class="pass__bg" src="../assets/img/rain_bk2.png" alt="background image" />
      <div class="pass__container">
        <div class="pass__label">Пройдите точный и быстрый</div>
        <h2 class="pass__title">тест на определение IQ</h2>
        <div class="pass__image"><img src="../assets/img/rain_bk3.png" alt="brain" /></div>
        <router-link to="/test" class="pass__button button">пройти тест</router-link>
        <div class="pass__text">
          <span>
            <p>И получите рекомендации</p>
            <p>по развитию своего интеллекта</p>
          </span>
          <p>и улучшению финансового</p>
          <p>благосостояния и личной жизни</p>
        </div>
        <div class="pass__actions actions">
          <div class="actions__icon">
            <img src="../assets/img/arrow.svg" alt="arrow" />
          </div>
          <div class="actions__text">Подробнее</div>
        </div>
      </div>
    </section>
    <section class="page__quote quote">
      <img class="quote__bg" src="../assets/img/quote.png" alt="background" />
      <div class="quote__container">
        <div class="quote__text">
          <img class="quote__decor1" src="../assets/img/quote-char.png" alt="decor" />
          <img class="quote__decor2" src="../assets/img/quote-char.png" alt="decor" />
          <!-- <p>“</p> -->
          <span
            >Профессиональный IQ-тест позволяет не только определить коэффициент вашего интеллекта,
            но и выработать список рекомендаций для повышения этого показателя.
          </span>
          <!-- <p>“</p> -->
        </div>
      </div>
    </section>
    <section class="page__tips tips">
      <div class="tips__container">
        <div class="tips__text">
          Также по результатам теста <span>вы получите</span> подробные <span>советы</span> по
          определению наиболее перспективной
          <span>для вашего типа интеллекта сферы деятельности</span>, которая принесет вам скорейший
          финансовый результат.
        </div>
        <div class="tips__image"><img src="../assets/img/tips.png" alt="tips" /></div>
        <router-link to="/test" class="tips__button button">пойти тест</router-link>
      </div>
    </section>
    <section class="page__try try">
      <img class="try__bg" src="../assets/img/rain_bk2.png" alt="background" />
      <div class="try__decor1"><img src="../assets/img/lightning_PNG9_1.png" alt="decor" /></div>
      <div class="try__decor2"><img src="../assets/img/lightning_PNG9.png" alt="decor" /></div>
      <div class="try__container">
        <div class="try__label">
          Прохождение теста займет у вас не более <span>12 минут</span>, а его результаты вы сможете
          <span>использовать всю жизнь.</span>
        </div>
        <div class="try__text">
          Профессиональная интерпретация и детально
          <span>проработанные рекомендации</span> позволят вам качественно
          <span>изменить все аспекты своей жизни:</span> от финансового до любовного.
        </div>
        <router-link to="/test" class="try__button button">пойти тест</router-link>
      </div>
    </section>
  </main>
</template>

<script>
export default {}
</script>

<style></style>
