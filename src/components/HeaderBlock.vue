<template>
  <header class="header">
    <div class="header__container">
      <div class="header__menu menu">
        <button type="button" class="menu__icon icon-menu">
          <span></span>
        </button>
        <nav class="menu__body">
          <ul class="menu__list">
            <li class="menu__item"><router-link to="/" class="menu__link">ГЛАВНАЯ</router-link></li>
            <li class="menu__item"><a href="" class="menu__link">ИНФОРМАЦИЯ О ТЕСТЕ</a></li>
            <li class="menu__item">
              <router-link to="/test" class="menu__link">ПРОЙТИ ТЕСТ</router-link>
            </li>
          </ul>
        </nav>
      </div>
    </div>
  </header>
</template>
<script>
export default {
  data() {
    document.addEventListener('click', function (e) {
      if (e.target.closest('.icon-menu') || e.target.closest('.menu__link')) {
        document.documentElement.classList.toggle('menu-open')
      }
    })

    return {}
  }
}
</script>

<style></style>
