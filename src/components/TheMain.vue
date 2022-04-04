<script setup>
import { computed, onMounted, reactive, ref } from 'vue'

let scrollTop = window.pageYOffset
const hiddenHeader = ref(false)
const isShownMobMenu = ref(false)
const classOpened = ref(false)
const productName = ref(null)
const quantity = ref(1)
const isBasket = ref(false)
const isFavorite = ref(false)
const isShownToast = ref(false)
let textInToast = ref('')
let textsToToast = reactive({})
const srcMainImage = ref('src/assets/image/14896420-2-1.jpg')
const srcImages = reactive([
  { img: 'src/assets/image/14896420-2-2.jpg' },
  { img: 'src/assets/image/14896420-1-1.jpg' },
  { img: 'src/assets/image/14896420-3-1.jpg' },
  { img: 'src/assets/image/14896420-4-1.jpg' },
  { img: 'src/assets/image/14896420-5-1.jpg' },
])
const inputSubscribe = ref('')

const btnFavColor = computed(() =>
  isFavorite.value
    ? 'red'
    : 'none'
)
const btnBasketText = computed(() =>
  isBasket.value
    ? 'Удалить из корзины'
    : 'Добавить в корзину'
)
const addBasketText = computed(() =>
  btnBasketText.value === 'Добавить в корзину'
    ? 'добавлен в корзину'
    : 'удален из корзины'
)
const addFavText = computed(() =>
  btnFavColor.value === 'red'
    ? 'удален из избранного'
    : 'добавлен в избранное'
)
const mainImage = computed(() =>
  srcMainImage.value === srcImages[0].img
    ? getImageUrl('src/assets/image/14896420-2-1.jpg')
    : getImageUrl(srcMainImage.value)
)

const onResize = () => (isShownMobMenu.value = window.innerWidth < 920)
const openMobMenu = () => {
  console.log(mouseX)
  classOpened.value = !classOpened.value
}
const onScroll = () => {
  hiddenHeader.value = window.pageYOffset > scrollTop
  scrollTop = window.pageYOffset
}
const changeQuantity = (click) => {
  if (quantity.value < 1) {
    return (quantity.value = 1)
  }
  if (click === 'plus') {
    quantity.value++
  } else if (click === 'minus') {
    quantity.value--
  }
}
const getRefEl = ({ el }) => productName.value = el.innerText
const addedTo = (to) => {
  textsToToast = {
    basket: `Товар "<strong>${ productName.value }</strong>" в количестве <strong>${ quantity.value }</strong> единиц ${ addBasketText.value }`,
    favorite: `Товар "<strong>${ productName.value }</strong>" в количестве <strong>${ quantity.value }</strong> единиц ${ addFavText.value }`,
  }

  if (quantity.value === 0) {
    return
  }

  if (to === 'basket') {
    isBasket.value = !isBasket.value

    textInToast.value = textsToToast.basket
    isShownToast.value = true

    setTimeout(() => {
      isShownToast.value = false
    }, 4000)
  }

  if (to === 'favorite') {
    isFavorite.value = !isFavorite.value

    textInToast.value = textsToToast.favorite
    isShownToast.value = true

    setTimeout(() => {
      isShownToast.value = false
    }, 4000)
  }
}
const getImageUrl = (name) => new URL(`/vue-alef-html/${ name }`, import.meta.url).href
const clickImage = (src) => srcMainImage.value = src
const clearInput = () => inputSubscribe.value = ''
const checkInputMail = (email) => {
  if (email) {
    const reg = /^([\w\d-]+\.)*[\w\d-]+@[\w\d-]+(\.[\w\d-]+)*\.[\w]{2,6}$/
    return !reg.test(email)
  }
  return false
}

onMounted(() => {
  onResize()
  window.addEventListener('resize', onResize)
  window.addEventListener('scroll', onScroll)
})
</script>

<template>
  <header
    :class="{ top_hidden: hiddenHeader }"
    class="header"
  >
    <div class="wrapper">
      <div class="content">
        <div class="logo">
          <a href="#">logo</a>
        </div>

        <div class="header__menu">
          <a href="#">
            <svg
              fill="none"
              height="16"
              viewBox="0 0 16 16"
              width="16"
              xmlns="http://www.w3.org/2000/svg"
            >
              <circle cx="8.45945" cy="4.18919" r="3.68919" stroke="#333333" />
              <path d="M0.5 15.3379H15.5" stroke="#333333" />
              <path
                d="M0.5 15.84C0.5 12.4347 4.84699 9.91339 8.21429 10.0023C11.4328 10.0872 15.5 12.4347 15.5 15.84"
                stroke="#333333"
              />
            </svg>
          </a>
          <a href="#">
            <svg
              fill="none"
              height="16"
              viewBox="0 0 16 16"
              width="16"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M7.90002 3.0948C6.69981 -2.52108 -2.59119 1.90389 1.76037 7.60175C6.11193 13.2996 8.40002 15.4908 8.40002 15.4908"
                stroke="#333333"
              />
              <path
                d="M7.89415 3.0948C9.09435 -2.52108 18.7304 1.90389 14.3789 7.60175C10.0273 13.2996 7.70001 15.5 7.70001 15.5"
                stroke="#333333"
              />
              <path
                d="M7.39099 2.70752L8.4043 2.70757L8.38337 3.24079H7.39099V2.70752Z"
                fill="#333333"
              />
            </svg>
          </a>
          <a href="#">
            <svg
              fill="none"
              height="17"
              viewBox="0 0 16 17"
              width="16"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M3.74689 4.86865H12.2531C13.7116 4.86865 14.8604 6.11201 14.7453 7.56595L14.2203 14.1975C14.1174 15.4977 13.0323 16.5002 11.7281 16.5002H4.27189C2.96769 16.5002 1.88261 15.4977 1.77968 14.1975L1.25468 7.56595C1.13958 6.11201 2.2884 4.86865 3.74689 4.86865Z"
                stroke="#333333"
              />
              <path
                d="M3.99999 9.5C3.99999 9.5 3.78329 1 8.00001 1C12.2167 1 12 9.5 12 9.5"
                stroke="#333333"
              />
            </svg>
          </a>
          <div v-show="isShownMobMenu" class="mob-menu">
            <div
              @click="openMobMenu"
              :class="{ opened: classOpened }"
              class="mob-menu__burger"
            >
              <span class="close"></span>
              <span></span>
              <span class="close"></span>
            </div>
            <ul :class="{ opened: classOpened }" class="sub-menu">
              <li><a href="#">постельное белье</a></li>
              <li><a href="#">одежда для дома</a></li>
              <li><a href="#">одежда для улицы</a></li>
              <li><a href="#">выход</a></li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>

  <main class="main">
    <div class="wrapper">
      <div class="content">
        <!--top-->
        <div class="block-top">
          <!--left-->
          <div class="block-left">
            <img :src="mainImage" alt="product" />
            <div class="img-preview">
              <a v-for="link in srcImages" :key="link" href="#">
                <img
                  @click="clickImage(link.img)"
                  :src="getImageUrl(link.img)"
                  alt="product"
                />
              </a>
            </div>
          </div>
          <!--right-->
          <div class="block-right">
            <div class="product-info">
              <!--title-->
              <div class="product-info header-text">
                <h2 @VnodeMounted="getRefEl" class="header-text list-title">
                  Пижама для девочек
                </h2>
                <small class="header-text subtext">Арт. 02765/46</small>

                <div class="header-text reviews">
                  <span class="reviews__title">Отзывы</span>
                  <span class="reviews__svg-stars">
                    <svg
                      fill="none"
                      height="12"
                      viewBox="0 0 12 12"
                      width="12"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M6 3.565L6.485 4.71L6.72 5.265L7.32 5.315L8.555 5.42L7.615 6.235L7.16 6.63L7.295 7.22L7.575 8.425L6.515 7.785L6 7.465L5.485 7.775L4.425 8.415L4.705 7.21L4.84 6.62L4.385 6.225L3.445 5.41L4.68 5.305L5.28 5.255L5.515 4.7L6 3.565V3.565ZM6 1L4.595 4.315L1 4.62L3.73 6.985L2.91 10.5L6 8.635L9.09 10.5L8.27 6.985L11 4.62L7.405 4.315L6 1Z"
                        fill="#333333"
                      />
                    </svg>
                    <svg
                      fill="none"
                      height="12"
                      viewBox="0 0 12 12"
                      width="12"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M6 3.565L6.485 4.71L6.72 5.265L7.32 5.315L8.555 5.42L7.615 6.235L7.16 6.63L7.295 7.22L7.575 8.425L6.515 7.785L6 7.465L5.485 7.775L4.425 8.415L4.705 7.21L4.84 6.62L4.385 6.225L3.445 5.41L4.68 5.305L5.28 5.255L5.515 4.7L6 3.565V3.565ZM6 1L4.595 4.315L1 4.62L3.73 6.985L2.91 10.5L6 8.635L9.09 10.5L8.27 6.985L11 4.62L7.405 4.315L6 1Z"
                        fill="#333333"
                      />
                    </svg>
                    <svg
                      fill="none"
                      height="12"
                      viewBox="0 0 12 12"
                      width="12"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M6 3.565L6.485 4.71L6.72 5.265L7.32 5.315L8.555 5.42L7.615 6.235L7.16 6.63L7.295 7.22L7.575 8.425L6.515 7.785L6 7.465L5.485 7.775L4.425 8.415L4.705 7.21L4.84 6.62L4.385 6.225L3.445 5.41L4.68 5.305L5.28 5.255L5.515 4.7L6 3.565V3.565ZM6 1L4.595 4.315L1 4.62L3.73 6.985L2.91 10.5L6 8.635L9.09 10.5L8.27 6.985L11 4.62L7.405 4.315L6 1Z"
                        fill="#333333"
                      />
                    </svg>
                    <svg
                      fill="none"
                      height="12"
                      viewBox="0 0 12 12"
                      width="12"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M6 3.565L6.485 4.71L6.72 5.265L7.32 5.315L8.555 5.42L7.615 6.235L7.16 6.63L7.295 7.22L7.575 8.425L6.515 7.785L6 7.465L5.485 7.775L4.425 8.415L4.705 7.21L4.84 6.62L4.385 6.225L3.445 5.41L4.68 5.305L5.28 5.255L5.515 4.7L6 3.565V3.565ZM6 1L4.595 4.315L1 4.62L3.73 6.985L2.91 10.5L6 8.635L9.09 10.5L8.27 6.985L11 4.62L7.405 4.315L6 1Z"
                        fill="#333333"
                      />
                    </svg>
                    <svg
                      fill="none"
                      height="12"
                      viewBox="0 0 12 12"
                      width="12"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M6 3.565L6.485 4.71L6.72 5.265L7.32 5.315L8.555 5.42L7.615 6.235L7.16 6.63L7.295 7.22L7.575 8.425L6.515 7.785L6 7.465L5.485 7.775L4.425 8.415L4.705 7.21L4.84 6.62L4.385 6.225L3.445 5.41L4.68 5.305L5.28 5.255L5.515 4.7L6 3.565V3.565ZM6 1L4.595 4.315L1 4.62L3.73 6.985L2.91 10.5L6 8.635L9.09 10.5L8.27 6.985L11 4.62L7.405 4.315L6 1Z"
                        fill="#333333"
                      />
                    </svg>
                  </span>
                  <span class="reviews__count">14 отзывов</span>
                  <a href="#">
                    <svg
                      fill="none"
                      height="24"
                      viewBox="0 0 24 24"
                      width="24"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path d="M8 5L16 12L8 18.5" stroke="#333333" />
                    </svg>
                  </a>
                </div>
              </div>

              <div class="product-info prices-block">
                <div class="price">
                  <span class="price__item price__item_new">800 ₽</span>
                  <span class="price__item price__item_old">1 500 ₽</span>
                  <a href="#">
                    <svg
                      fill="none"
                      height="24"
                      viewBox="0 0 24 24"
                      width="24"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path d="M8 5L16 12L8 18.5" stroke="#333333" />
                    </svg>
                  </a>
                </div>

                <div class="badge">
                  <span class="badge__item">скидка -36%</span>
                  <span class="badge__item">акция -20%</span>
                </div>
              </div>
              <!--form-->
              <div class="product-info buy">
                <form action="#" id="form-buy" method="post">
                  <fieldset class="select">
                    <select
                      class="select-size"
                      form="form-buy"
                      name="select-size"
                    >
                      <option disabled selected>Выбрать размер</option>
                      <option value="value1">Значение 1</option>
                      <option value="value2">Значение 2</option>
                      <option value="value3">Значение 3</option>
                    </select>

                    <div class="link_underline">
                      <a href="#">Определить размер</a>
                    </div>
                  </fieldset>

                  <fieldset class="basket">
                    <div class="buttons-block">
                      <div class="quantity-block">
                        <button
                          @click="changeQuantity('plus')"
                          class="button button_light"
                          type="button"
                        >
                          +
                        </button>
                        <input
                          v-model="quantity"
                          :placeholder="quantity"
                          class="quantity"
                          type="text"
                        />
                        <button
                          @click="changeQuantity('minus')"
                          class="button button_light"
                          type="button"
                        >
                          -
                        </button>
                      </div>

                      <button
                        @click.prevent="addedTo('basket')"
                        autofocus
                        class="button button_text button_black"
                        type="button"
                      >
                        {{ btnBasketText }}
                      </button>

                      <button
                        @click="addedTo('favorite')"
                        class="button button_icon button_black"
                        type="button"
                      >
                        <svg
                          :fill="btnFavColor"
                          height="24"
                          viewBox="0 0 24 24"
                          width="24"
                          xmlns="http://www.w3.org/2000/svg"
                        >
                          <path
                            d="M11.9 7.0948C10.6998 1.47892 1.40881 5.90389 5.76037 11.6018C10.1119 17.2996 12.4 19.4908 12.4 19.4908"
                            stroke="white"
                          />
                          <path
                            d="M11.8941 7.0948C13.0944 1.47892 22.7304 5.90389 18.3789 11.6018C14.0273 17.2996 11.7 19.5 11.7 19.5"
                            stroke="white"
                          />
                          <path
                            d="M11.391 6.70752L12.4043 6.70757L12.3834 7.24079H11.391V6.70752Z"
                            fill="white"
                          />
                        </svg>
                      </button>

                      <div v-if="isShownToast" class="toast">
                        <p class="toast__text" v-html="textInToast"></p>
                      </div>
                    </div>

                    <div class="link_underline">
                      <a href="#">Купить в 1 клик</a>
                    </div>
                  </fieldset>
                </form>
              </div>
              <!--describe-->
              <div class="product-info describe">
                <a href="#">
                  <svg
                    fill="none"
                    height="20"
                    viewBox="0 0 20 20"
                    width="20"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <circle cx="10" cy="10" r="7" stroke="#333333" />
                    <path
                      d="M6 8C6.24928 8 8.75787 6.5 8.75787 6.5H10.0043H11.2507C11.2507 6.5 13.7507 8 14 8"
                      stroke="#333333"
                    />
                    <path
                      d="M6.01577 7.39283L6.01576 9.17806L7.76075 9.4756L7.76075 13.9386"
                      stroke="#333333"
                    />
                    <path
                      d="M13.9842 7.39283L13.9842 9.17806L12.2393 9.4756L12.2478 13.9386"
                      stroke="#333333"
                    />
                    <line
                      stroke="#333333"
                      x1="7.51145"
                      x2="12.4971"
                      y1="13.4385"
                      y2="13.4385"
                    />
                    <path
                      d="M8.50859 6.5C8.50859 6.5 9.00716 8.28522 10.0043 8.28522C11.0014 8.28522 11.5 6.5 11.5 6.5"
                      stroke="#333333"
                    />
                  </svg>
                  Описание товара
                </a>
                <a href="#">
                  <svg
                    fill="none"
                    height="20"
                    viewBox="0 0 20 20"
                    width="20"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <circle
                      cx="10"
                      cy="10"
                      fill="white"
                      r="7"
                      stroke="#333333"
                    />
                    <path
                      d="M9.55453 5.5L9.55453 10.7061L13.5 13"
                      stroke="#333333"
                    />
                  </svg>
                  Доставка и возврат
                </a>
                <a href="#">
                  <svg
                    fill="none"
                    height="20"
                    viewBox="0 0 20 20"
                    width="20"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <rect
                      height="9"
                      rx="0.5"
                      stroke="#333333"
                      width="13"
                      x="3.5"
                      y="5.5"
                    />
                    <rect fill="#333333" height="2" width="12" x="4" y="8" />
                  </svg>
                  Способы оплаты
                </a>
              </div>
            </div>
          </div>
        </div>
        <!--bottom-->
        <div class="block-bottom">
          <div class="subtitle">
            <a href="#">Посмотреть все стили</a>
          </div>

          <div class="block-left">
            <img alt="pic" src="../assets/image/158-0.jpg" />
          </div>

          <div class="block-right">
            <a href="#">
              <div class="hidden">
                <svg
                  class="icon-pay"
                  fill="none"
                  height="24"
                  viewBox="0 0 24 24"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect
                    height="13"
                    rx="1.5"
                    stroke="white"
                    width="13"
                    x="4.5"
                    y="6.5"
                  />
                  <path
                    d="M6.33784 6.63126L6.33784 6.49958C6.33784 5.39501 7.23327 4.49958 8.33784 4.49958L17.5 4.49959C18.6045 4.49959 19.5 5.39502 19.5 6.49959L19.5 15.9382C19.5 16.9853 18.5159 17.7537 17.5 17.4997V17.4997"
                    stroke="white"
                  />
                </svg>

                <div class="product-info">
                  <div class="more-info">
                    <svg
                      fill="none"
                      height="48"
                      viewBox="0 0 48 48"
                      width="48"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M8.75216 16.9953C8.63818 15.542 9.78674 14.2998 11.2445 14.2998H36.7555C38.2133 14.2998 39.3618 15.542 39.2478 16.9953L37.5851 38.1953C37.4831 39.4962 36.3976 40.4998 35.0927 40.4998H12.9073C11.6024 40.4998 10.5169 39.4962 10.4149 38.1953L8.75216 16.9953Z"
                        stroke="white"
                      />
                      <path
                        d="M15.004 24.4857C15.004 24.4857 14.5437 7 23.5 7C32.4563 7 31.996 24.4857 31.996 24.4857"
                        stroke="white"
                      />
                    </svg>
                    <span>Узнай, что на мне</span>
                  </div>

                  <div class="likes">
                    <svg
                      fill="white"
                      height="24"
                      viewBox="0 0 24 24"
                      width="24"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M11.9 7.0948C10.6998 1.47892 1.40881 5.90389 5.76037 11.6018C10.1119 17.2996 12.4 19.4908 12.4 19.4908"
                        stroke="white"
                      />
                      <path
                        d="M11.8941 7.0948C13.0944 1.47892 22.7304 5.90389 18.3789 11.6018C14.0273 17.2996 11.7 19.5 11.7 19.5"
                        stroke="white"
                      />
                      <path
                        d="M11.391 6.70752L12.4043 6.70757L12.3834 7.24079H11.391V6.70752Z"
                        fill="white"
                      />
                    </svg>
                    <span>200</span>
                  </div>
                </div>
              </div>
              <img alt="pic" src="../assets/image/158-1.jpg" />
            </a>

            <a href="#">
              <div class="hidden">
                <svg
                  class="icon-pay"
                  fill="none"
                  height="24"
                  viewBox="0 0 24 24"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect
                    height="13"
                    rx="1.5"
                    stroke="white"
                    width="13"
                    x="4.5"
                    y="6.5"
                  />
                  <path
                    d="M6.33784 6.63126L6.33784 6.49958C6.33784 5.39501 7.23327 4.49958 8.33784 4.49958L17.5 4.49959C18.6045 4.49959 19.5 5.39502 19.5 6.49959L19.5 15.9382C19.5 16.9853 18.5159 17.7537 17.5 17.4997V17.4997"
                    stroke="white"
                  />
                </svg>

                <div class="product-info">
                  <div class="more-info">
                    <svg
                      fill="none"
                      height="48"
                      viewBox="0 0 48 48"
                      width="48"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M8.75216 16.9953C8.63818 15.542 9.78674 14.2998 11.2445 14.2998H36.7555C38.2133 14.2998 39.3618 15.542 39.2478 16.9953L37.5851 38.1953C37.4831 39.4962 36.3976 40.4998 35.0927 40.4998H12.9073C11.6024 40.4998 10.5169 39.4962 10.4149 38.1953L8.75216 16.9953Z"
                        stroke="white"
                      />
                      <path
                        d="M15.004 24.4857C15.004 24.4857 14.5437 7 23.5 7C32.4563 7 31.996 24.4857 31.996 24.4857"
                        stroke="white"
                      />
                    </svg>
                    <span>Узнай, что на мне</span>
                  </div>

                  <div class="likes">
                    <svg
                      fill="white"
                      height="24"
                      viewBox="0 0 24 24"
                      width="24"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M11.9 7.0948C10.6998 1.47892 1.40881 5.90389 5.76037 11.6018C10.1119 17.2996 12.4 19.4908 12.4 19.4908"
                        stroke="white"
                      />
                      <path
                        d="M11.8941 7.0948C13.0944 1.47892 22.7304 5.90389 18.3789 11.6018C14.0273 17.2996 11.7 19.5 11.7 19.5"
                        stroke="white"
                      />
                      <path
                        d="M11.391 6.70752L12.4043 6.70757L12.3834 7.24079H11.391V6.70752Z"
                        fill="white"
                      />
                    </svg>
                    <span>200</span>
                  </div>
                </div>
              </div>
              <img alt="pic" src="../assets/image/158-2.jpg" />
            </a>

            <a href="#">
              <div class="hidden">
                <svg
                  class="icon-pay"
                  fill="none"
                  height="24"
                  viewBox="0 0 24 24"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect
                    height="13"
                    rx="1.5"
                    stroke="white"
                    width="13"
                    x="4.5"
                    y="6.5"
                  />
                  <path
                    d="M6.33784 6.63126L6.33784 6.49958C6.33784 5.39501 7.23327 4.49958 8.33784 4.49958L17.5 4.49959C18.6045 4.49959 19.5 5.39502 19.5 6.49959L19.5 15.9382C19.5 16.9853 18.5159 17.7537 17.5 17.4997V17.4997"
                    stroke="white"
                  />
                </svg>

                <div class="product-info">
                  <div class="more-info">
                    <svg
                      fill="none"
                      height="48"
                      viewBox="0 0 48 48"
                      width="48"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M8.75216 16.9953C8.63818 15.542 9.78674 14.2998 11.2445 14.2998H36.7555C38.2133 14.2998 39.3618 15.542 39.2478 16.9953L37.5851 38.1953C37.4831 39.4962 36.3976 40.4998 35.0927 40.4998H12.9073C11.6024 40.4998 10.5169 39.4962 10.4149 38.1953L8.75216 16.9953Z"
                        stroke="white"
                      />
                      <path
                        d="M15.004 24.4857C15.004 24.4857 14.5437 7 23.5 7C32.4563 7 31.996 24.4857 31.996 24.4857"
                        stroke="white"
                      />
                    </svg>
                    <span>Узнай, что на мне</span>
                  </div>

                  <div class="likes">
                    <svg
                      fill="white"
                      height="24"
                      viewBox="0 0 24 24"
                      width="24"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M11.9 7.0948C10.6998 1.47892 1.40881 5.90389 5.76037 11.6018C10.1119 17.2996 12.4 19.4908 12.4 19.4908"
                        stroke="white"
                      />
                      <path
                        d="M11.8941 7.0948C13.0944 1.47892 22.7304 5.90389 18.3789 11.6018C14.0273 17.2996 11.7 19.5 11.7 19.5"
                        stroke="white"
                      />
                      <path
                        d="M11.391 6.70752L12.4043 6.70757L12.3834 7.24079H11.391V6.70752Z"
                        fill="white"
                      />
                    </svg>
                    <span>200</span>
                  </div>
                </div>
              </div>
              <img alt="pic" src="../assets/image/158-3.jpg" />
            </a>

            <a href="#">
              <div class="hidden">
                <svg
                  class="icon-pay"
                  fill="none"
                  height="24"
                  viewBox="0 0 24 24"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect
                    height="13"
                    rx="1.5"
                    stroke="white"
                    width="13"
                    x="4.5"
                    y="6.5"
                  />
                  <path
                    d="M6.33784 6.63126L6.33784 6.49958C6.33784 5.39501 7.23327 4.49958 8.33784 4.49958L17.5 4.49959C18.6045 4.49959 19.5 5.39502 19.5 6.49959L19.5 15.9382C19.5 16.9853 18.5159 17.7537 17.5 17.4997V17.4997"
                    stroke="white"
                  />
                </svg>

                <div class="product-info">
                  <div class="more-info">
                    <svg
                      fill="none"
                      height="48"
                      viewBox="0 0 48 48"
                      width="48"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M8.75216 16.9953C8.63818 15.542 9.78674 14.2998 11.2445 14.2998H36.7555C38.2133 14.2998 39.3618 15.542 39.2478 16.9953L37.5851 38.1953C37.4831 39.4962 36.3976 40.4998 35.0927 40.4998H12.9073C11.6024 40.4998 10.5169 39.4962 10.4149 38.1953L8.75216 16.9953Z"
                        stroke="white"
                      />
                      <path
                        d="M15.004 24.4857C15.004 24.4857 14.5437 7 23.5 7C32.4563 7 31.996 24.4857 31.996 24.4857"
                        stroke="white"
                      />
                    </svg>
                    <span>Узнай, что на мне</span>
                  </div>

                  <div class="likes">
                    <svg
                      fill="white"
                      height="24"
                      viewBox="0 0 24 24"
                      width="24"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M11.9 7.0948C10.6998 1.47892 1.40881 5.90389 5.76037 11.6018C10.1119 17.2996 12.4 19.4908 12.4 19.4908"
                        stroke="white"
                      />
                      <path
                        d="M11.8941 7.0948C13.0944 1.47892 22.7304 5.90389 18.3789 11.6018C14.0273 17.2996 11.7 19.5 11.7 19.5"
                        stroke="white"
                      />
                      <path
                        d="M11.391 6.70752L12.4043 6.70757L12.3834 7.24079H11.391V6.70752Z"
                        fill="white"
                      />
                    </svg>
                    <span>200</span>
                  </div>
                </div>
              </div>
              <img alt="pic" src="../assets/image/158-4.jpg" />
            </a>
          </div>
        </div>
      </div>
    </div>
  </main>

  <footer class="footer">
    <div class="wrapper">
      <div class="content">
        <div class="list-block">
          <h3 class="list-title">покупателям</h3>
          <ul class="list">
            <li class="list__item"><a href="#">Каталог</a></li>
            <li class="list__item"><a href="#">Акции</a></li>
            <li class="list__item"><a href="#">Бренды</a></li>
          </ul>
        </div>
        <div class="list-block">
          <h3 class="list-title">о нас</h3>
          <ul class="list">
            <li class="list__item"><a href="#">О компании</a></li>
            <li class="list__item"><a href="#">Новости</a></li>
            <li class="list__item"><a href="#">Команда</a></li>
          </ul>
        </div>
        <div class="promo">
          <h3 class="title">Узнайте первыми о новинках и акциях</h3>
          <div class="input-block">
            <input
              class="input-mail"
              placeholder="Адрес электронной почты"
              type="email"
              v-model.trim.lazy="inputSubscribe"
            />
            <svg
              v-if="inputSubscribe"
              @click="clearInput"
              class="icon-close"
              fill="none"
              height="16"
              viewBox="0 0 16 16"
              width="16"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path d="M3.70001 12.0664L12.0501 3.88337" stroke="#C4C4C4" />
              <path d="M12.2516 12.0664L3.90161 3.88335" stroke="#C4C4C4" />
            </svg>
            <div
              v-if="checkInputMail(inputSubscribe)"
              class="toast email"
            >
              Email неподходящего вида: name@mail.com
            </div>
          </div>
          <button class="button button_white">
            Подписаться
          </button>
        </div>
      </div>
    </div>
  </footer>
</template>

<style lang="scss">
.logo {
  color: #000;
  font-size: 22px;
  font-weight: 700;
  line-height: 16px;
  letter-spacing: 0.04em;
  text-transform: uppercase;

  a {
    text-decoration: none;
  }
}

.toast {
  position: absolute;
  right: 20px;
  max-width: 55%;
  height: auto;
  margin-top: 60px;
  margin-left: auto;
  padding: 10px;
  background: #d9d9d9;
  border: 1px solid #000;
  opacity: 0.8;
  z-index: 40;

  &.email {
    top: 0;
    left: 0;
    margin-left: 0;
  }

  @media (max-width: 395px) {
    left: 0;
    margin-top: 110px;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.header {
  position: sticky;
  top: 0;
  height: 40px;
  padding: 12px 0;
  background: #fff;
  transition: all ease-in 0.7s;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  z-index: 20;

  &.top_hidden {
    top: -40px;

    @media (max-width: 920px) {
      top: 0;
    }
  }

  &__menu {
    position: relative;
    display: flex;
    justify-content: space-between;
    min-width: 93px;
    margin-right: 7px;

    @media (max-width: 920px) {
      min-width: 168px;
    }
  }

  .mob-menu {
    position: relative;

    &__burger {
      position: relative;
      display: block;
      width: 18px;
      height: 18px;
      cursor: pointer;
      transition: opacity 0.2s linear;

      span {
        position: absolute;
        top: 1px;
        right: 1px;
        display: block;
        width: 18px;
        height: 1px;
        background-color: #333;
        transition: all 0.4s ease;
        overflow: hidden;

        &:nth-child(1) {
          margin-top: 0;
          z-index: 9;
        }

        &:nth-child(2) {
          margin-top: 6px;
          z-index: 9;
        }

        &:nth-child(3) {
          margin-top: 12px;
          z-index: 9;
        }
      }
    }

    .sub-menu {
      position: fixed;
      top: 0;
      left: 0;
      display: none;
      width: 100%;
      padding: 80px 30px 0;
      font-size: 12px;
      line-height: 16px;
      background-color: #fff;
      text-transform: uppercase;
      transition: all 4s ease;
      z-index: 21;

      li {
        padding-bottom: 30px;

        &:last-child {
          padding-bottom: 43px;
        }
      }

      &.opened {
        display: block;
        transition: all 4s ease;
      }
    }

    .opened {
      span:nth-child(1),
      span:nth-child(3) {
        z-index: 23;
      }

      span:nth-child(1) {
        transform: rotate(45deg) translateY(4px) translateX(4px);
      }

      span:nth-child(2) {
        width: 0;
        height: 0;
      }

      span:nth-child(3) {
        transform: rotate(-45deg) translateY(-4px) translateX(4px);
      }
    }
  }

  @media (max-width: 1389px) {
    padding-top: 12px;

    .content {
      padding: 0 20px;
    }
  }

  @media (max-width: 920px) {
    height: 67px;
    padding: 22px 20px 0;
    box-shadow: none;

    .content {
      padding: 0;
    }
  }
}

.main {
  min-height: 880px;
  padding-top: 24px;
  @media (max-width: 375px) {
    padding-top: 0;
  }

  .block-top {
    display: flex;
    flex-wrap: wrap;
    width: 100%;

    .block-left {
      position: relative;
      flex: 0 1 50%;
      max-width: 686px;
      max-height: 878px;
      @media (max-width: 920px) {
        flex: 0 1 auto;
      }

      .img-preview {
        position: absolute;
        top: 26px;
        left: 21px;
        width: 70px;
        height: 91px;
        @media (max-width: 420px) {
          left: 24px;
          width: 40px;
          height: 52px;
        }
      }
    }

    .block-right {
      margin-left: 16px;
      @media (max-width: 920px) {
        margin-left: 0;
      }

      .product-info {
        position: relative;

        &.header-text {
          padding: 24px 20px 26px;

          .list-title {
            margin-bottom: 8px;
          }

          .subtext {
            display: block;
            margin-bottom: 16px;
            color: #828282;
            font-size: 12px;
            line-height: 16px;
          }

          .reviews {
            display: flex;
            align-items: center;

            a {
              display: flex;
            }

            &__title {
              margin-right: 12px;
            }

            &__svg-stars {
              margin-top: 3px;
              margin-right: 4px;
            }
          }
        }

        &.prices-block {
          padding: 20px;

          .price {
            display: flex;
            align-items: center;
            margin-bottom: 12px;

            &__item {
              margin-right: 12px;

              &_new {
                font-size: 24px;
                font-weight: 700;
                line-height: 24px;
                text-transform: uppercase;
              }

              &_old {
                color: #828282;
                font-size: 14px;
                line-height: 20px;
                text-decoration-line: line-through;
              }
            }
          }

          .badge {
            &__item {
              margin-right: 10px;
              padding: 4px 8px;
              font-size: 12px;
              line-height: 16px;
              border: 1px solid #000;
            }
          }
        }

        &.buy {
          padding: 20px;

          .select {
            margin-bottom: 40px;

            &-size {
              width: 315px;
              height: 44px;
              margin-bottom: 12px;
              padding: 10px;
              background-color: #fff;
              background-image: url("../assets/image/icon/arrow_down_swipe.svg");
              background-repeat: no-repeat;
              background-position: right 14px top 50%;
              border: 1px solid #333;
              appearance: none;
            }
          }

          .basket {
            .buttons-block {
              display: flex;
              align-items: stretch;
              min-height: 44px;
              margin-bottom: 12px;
              @media (max-width: 920px) {
                flex-wrap: wrap;
              }

              .quantity-block {
                display: flex;
                justify-content: space-between;
                align-items: stretch;
                width: 145px;
                margin-right: 12px;
                background: #f2f2f2;

                .quantity {
                  align-self: center;
                  width: 100%;
                  background: #f2f2f2;
                  border: none;
                  text-align: center;
                  outline: none;
                  overflow-block: hidden;
                }

                @media (max-width: 437px) {
                  margin-bottom: 15px;
                }
              }

              .button_text {
                margin-right: 4px;
              }
            }
          }
        }
      }

      .describe {
        display: flex;
        flex-direction: column;
        padding: 24px 20px;
        border-top: 0.5px solid #c4c4c4;

        a {
          display: flex;
          margin-bottom: 12px;

          svg {
            margin-right: 4px;
          }
        }
      }
    }
  }

  .block-bottom {
    display: grid;
    gap: 16px;
    place-items: center;
    @media (max-width: 920px) {
      gap: 8px;
    }

    .subtitle {
      grid-column: 1/5;
      padding: 32px 0 16px;
      text-align: center;
      @media (max-width: 920px) {
        grid-column: 1/3;
        padding: 58px 0 51px;
      }
    }

    .block-left {
      grid-column: 1/3;
      @media (max-width: 920px) {
        grid-row: 2/3;
        grid-column: 1/3;
      }
    }

    .block-right {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      grid-column: 3/5;
      gap: 16px;
      padding-top: 0;
      @media (max-width: 920px) {
        grid-column: 1/3;
        gap: 8px;
      }

      a {
        position: relative;
        flex: 0 1 auto;

        .hidden {
          display: none;
        }

        &:hover {
          .hidden {
            position: absolute;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
            padding: 16px;
            color: #fff;
            background: rgba(51, 51, 51, 0.7);
            animation: soft-opacity ease-in 0.35s;

            .icon-pay {
              align-self: flex-end;
            }

            .product-info {
              display: flex;
              flex-direction: column;
              justify-content: space-between;
              align-items: center;
              min-height: 60%;

              .more-info {
                display: flex;
                flex-direction: column;
                align-items: center;
              }
            }

            .likes {
              display: flex;
              align-items: center;
            }

            @media (max-width: 375px) {
              display: none;
            }
          }

          @keyframes soft-opacity {
            from {
              opacity: 0;
            }
            to {
              opacity: 100%;
            }
          }
        }
      }
    }
  }
}

.footer {
  margin: 130px 70px;
  @media (max-width: 1261px) {
    margin: 30px 20px 186px 18px;
  }

  .wrapper {
    max-width: 1154px;
  }

  .content {
    align-items: flex-start;
    padding-top: 12px;

    .list-block {
      flex: 1 1 335px;
      @media (max-width: 1261px) {
        flex: 1 1 50%;
      }

      .list-title {
        padding-bottom: 16px;
        text-transform: uppercase;
      }

      .list__item {
        margin-bottom: 8px;

        a {
          text-decoration: none;
        }
      }
    }

    .promo {
      display: flex;
      flex: 0 1 452px;
      flex-direction: column;
      justify-content: space-between;
      align-items: flex-start;
      @media (max-width: 1261px) {
        margin-top: 58px;
      }

      .title {
        margin-bottom: 33px;
        text-transform: uppercase;
        @media (max-width: 490px) {
          width: 200px;
        }
      }

      .input-block {
        position: relative;
        width: 100%;
        margin-bottom: 35px;

        .input-mail {
          width: 100%;
          padding: 12px 16px;
          color: #828282;
          border-top: none;
          border-right: none;
          border-bottom: 1px solid #c4c4c4;
          border-left: none;
          outline: none;
        }

        .icon-close {
          position: absolute;
          top: 12px;
          right: 8px;
          cursor: pointer;
        }
      }

      .button {
        align-self: flex-end;
        width: 150px;
        height: 36px;
      }
    }
  }
}
</style>
