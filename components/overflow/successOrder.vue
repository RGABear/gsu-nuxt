<template>
  <Overflow class="order-overflow">
    <img
      class="close"
      @click="emits('close')"
      src="~/assets/svg/close.svg"
      alt="close"
    />

    <div class="overflow__content">
      <svg
        width="108"
        height="108"
        viewBox="0 0 108 108"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <g filter="url(#filter0_d_3167_35844)">
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M54 94C76.0914 94 94 76.0914 94 54C94 31.9086 76.0914 14 54 14C31.9086 14 14 31.9086 14 54C14 76.0914 31.9086 94 54 94ZM54 89.8333C73.7902 89.8333 89.8333 73.7902 89.8333 54C89.8333 34.2098 73.7902 18.1667 54 18.1667C34.2098 18.1667 18.1667 34.2098 18.1667 54C18.1667 73.7902 34.2098 89.8333 54 89.8333Z"
            fill="#129DF4"
          />
        </g>
        <path
          d="M38.166 53.1667L49.8327 64.8333L70.666 44"
          stroke="#129DF4"
          stroke-width="5"
          stroke-linecap="round"
        />
        <defs>
          <filter
            id="filter0_d_3167_35844"
            x="0.25"
            y="0.25"
            width="107.5"
            height="107.5"
            filterUnits="userSpaceOnUse"
            color-interpolation-filters="sRGB"
          >
            <feFlood flood-opacity="0" result="BackgroundImageFix" />
            <feColorMatrix
              in="SourceAlpha"
              type="matrix"
              values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"
              result="hardAlpha"
            />
            <feOffset />
            <feGaussianBlur stdDeviation="6.875" />
            <feComposite in2="hardAlpha" operator="out" />
            <feColorMatrix
              type="matrix"
              values="0 0 0 0 0.223529 0 0 0 0 0.701961 0 0 0 0 0.956863 0 0 0 0.96 0"
            />
            <feBlend
              mode="normal"
              in2="BackgroundImageFix"
              result="effect1_dropShadow_3167_35844"
            />
            <feBlend
              mode="normal"
              in="SourceGraphic"
              in2="effect1_dropShadow_3167_35844"
              result="shape"
            />
          </filter>
        </defs>
      </svg>
      <div class="column column_gap16">
        <div class="text text_h3 text_center">Ваш заказ успешно создан!</div>
        <div class="text text_normal text_center">
          Информация о заказке выслана вам на почту <b>{{ mail }}</b
          >. Наш менеджер свяжется с вами в ближайшее время для уточнения
          данных.
        </div>
      </div>

      <div class="order-overflow__buttons">
        <button @click="emits('toAuth')" class="button button_black-bordered">
          Заполнить данные в профиле
        </button>
        <NuxtLink :to="payLink" target="_blank" style="width: 100%">
          <button class="button button_dark">
            Оплатить заказ ({{ sum }} ₽)
          </button>
        </NuxtLink>
      </div>
    </div>
  </Overflow>
</template>

<script setup>
const emits = defineEmits(["close", "toAuth"]);
const sum = useState("orderSum");
const mail = useState("orderMail");
const payLink = useState("orderLink");
</script>

<style scoped lang="less">
@import "assets/core.less";

.order-overflow {
  width: 100%;

  @media @min1200 {
    width: 1000px;
  }
}

.order-overflow__buttons {
  display: flex;
  flex-direction: column;
  width: 100%;
  gap: 10px;

  @media @min760 {
    flex-direction: row;
    max-width: 620px;
  }
}

.overflow__content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 40px;
}

.close {
  position: absolute;
  top: 20px;
  right: 20px;
  cursor: pointer;
}
</style>
